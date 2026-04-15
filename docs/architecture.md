# Architecture

At it's core, dfapi is a REST-like server with a transaction-like nature of requests. The server is based on my [Plugin Server](https://pluginserver.readthedocs.io).

The BasePlugin class is extended to ApiPlugin. 
Objects of ApiPlugin are wrapped in a dfRquest object. 
The request object produces a clean and well structured response envelope with the response data within. 

## Request object attributes

|Proerty | Use |
|--------|-----|
|client_ip| IP Address of client|
|code| Default to 200, set the code of the http response|
|cookie_data| Any associated cookie data|
|dns_name| The real dns name of the node connected to|
|elapsed| The elapsed time after the module's request method was called|
|handler| callable: The modules request method |
|keyname| The name of the key in the envelope to return data, default is data.|
|log| A plugincore.LogJam object for use by the module. |
|modinfo| A str to identify the module name and version.|
|node_name| The node name of the server.|
|request_path| The full request path minus address and protocol.|
|request_time| The time of the request.|
|subpath| The module relative endpoint path.|
|ticket| a Ticket object from ticketStore.|
|ticket_id| The ticket id of the transaction.|
|uuid| The UUID of the transaction|

ApiPlugin performs the request by calling dfRequest.new() with parameters to create rhe request object. These are passed on from the BasePlugin superclass and the pluginserver's request module. The return value from new is a fully addressed envelope containing the module return data, error data and the appropriate response format for BasePlugin.

This scheme satisfies several needs:

* Consistent request and response format.
* Leaves the modules core function logic the focus of the module.
* Performs all needed security checks ahead of time (ticket validation, user roles, etc.) without which the request never gets made.

For role checking the the request object's ticket property can be used to test a symbolic name with `rq.ticket.is_role(name:str)`. The module is free to create one so long as users are granted that role.

The basic signature for the request module is: 

```python
    async def request(self,rq, **kwargs):
```

`rq` is the request transaction object and `**kargs` contains get/post data, request headers, the aiohttp request object itself. 

<details>
<summary><b>Implementation</b></summary>

```python
class ApiPlugin(BasePlugin):
    def __init__(self,**kwargs):
        super().__init__(**kwargs)
        self.node_name = os.uname().nodename
        self.dns_name = dfdns.get_fqdn(self.node_name.split('.')[0])
        if not hasattr(self,'modinfo'):
            self.modinfo = kwargs.get('modinfo')
        if not self.modinfo:
            raise ValueError("modinfo is required for ApiPlug")

    @staticmethod
    def request(self,**kwargs):
        pass
    
    async def request_handler(self, **kwargs):
        return await dfRequest.new(
                code = 200,
                modinfo=self.modinfo,
                request_path=self._plugin_id, 
                handler=self.request,
                **kwargs
            )

```

The dfRequest class creates a request specific uuid that can be used in the request handler. A number of attributes are built in. See below and also the [attributes](#Request-object-attributes) table above.


```python
class dfRequest:
    def __init__(self,**kwargs):
        pass

    @classmethod
    async def new(cls, *, modinfo=DEFAULT_MODINFO, code=200, request_path=None,
                  handler=None, keyname="data",cookie_data=None, no_ticket_ok=False, 
                  **kwargs):
        self = cls()
        self.uuid = str(uuid.uuid4())
        self.modinfo = modinfo if modinfo else self.modinfo
        self.code = code
        self.request_path = request_path
        self.keyname = keyname
        self.cookie_data = cookie_data
        self.client_ip = kwargs.get('client_ip')
        self.ticket_id = kwargs.get('ticket_id')
        self.subpath = kwargs.get('subpath',)
        self.node_name = os.uname().nodename
        self.dns_name = dfdns.get_fqdn(self.node_name.split('.')[0])
        self.elapsed = 0
        self.request_time = int(time.time())
        self.handler = handler
        self.log = kwargs.get('log')
        if not self.modinfo and self._plugin_id:
            self.modinfo = f"com.ducksfeet.dfapi.{self.plugin_id}(synthesized):v0.0.0"
        if not handler:
            raise ValueError("handler is required for dfRequest")
        try:
            self.ticket = await tickets.Ticket.new(ticket_id=self.ticket_id, ip_address=self.client_ip) if self.ticket_id and self.client_ip else None
        except Exception as e:
            if not no_ticket_ok:
                raise e
            self.ticket = None
    
        try:
            time_in = time.time()
            self.data = await self.handler(self, **kwargs)
            time_out = time.time()
            self.elapsed = time_out - time_in
        except Exception as e:
            self.code = 500
            self.data = f"Exception {type(e).__name__}: {str(e)}"
        return await self.to_response()

    async def to_response(self):
        request_path = "no-request-path" if not self.request_path else self.request_path
        if isinstance(self.data,dict):
            self.log.debug(f"Setting uuid")
            self.data['_uuid'] = self.uuid
        else:
            self.log.debug(f"Cannot add uuid to data data is {type(self.data).__name__}")
        request_path = '/'.join([request_path,self.subpath]) if self.subpath else request_path
        rd = {
            "_id": self.uuid,
            "status": "ok" if self.code >= 200 and self.code < 300 else "error",
            "modinfo": self.modinfo,
            "server_info": {
                "api_server_name": self.dns_name,
                "api_server_addr": socket.gethostbyname(self.dns_name),
                "physical_node": self.node_name,
                "physical_address": socket.gethostbyname(self.node_name),
            },
            "request": {
                "_id": self.uuid,
                "request_path": request_path,
                "request_time": self.request_time,
                "response_time": self.elapsed,
                "client": {
                    "ip": self.client_ip,
                    "node": dfdns.resolve_ip(self.client_ip),
                },
            },
            self.keyname: self.data if self.data is not None else None
        }
        response = web.json_response(rd,status=self.code)
        if self.code == 200:
            if self.data:
                try:
                    if self.cookie_data:
                        response.set_cookie(**self.cookie_data)
                    if self.log:
                        self.log.debug(f"api_response: 200 with data response {rd}")
                    return 200, response
                except Exception as e:
                    response = web.json_response({'status': 'error', 'request_path': request_path, 'message': f"Exception {type(e).__name__}: {e}"}, status=500)
                    if self.log:
                        self.log.error(f"api_response: 200 with data exception {type(e).__name__}: {e}")
                    return 500, response
            else:
                response = web.json_response(rd,status=self.code)
                if self.log:
                    self.log.debug(f"api_response: 200 no data response")
                return self.code, web.json_response(rd,status=self.code)
        response = web.json_response({'status': 'error', 'request_path': request_path, 'message': self.data}, status=self.code)
        if self.log:
            self.log.debug(f"api_response: {self.code} response {rd}")
        return self.code, response

```
</details>
