# Fundamentals of API Calls

The API server is REST-like server with more flexible data handling and less stringent request methods.

Calling the API is a, at the most basic level a HTTP request.

All requests return HTTP status codes of 2xx on and 4xx/5xx when errors are encountered. On a successful call, a response envelope is returned with transactional information in the response 'envelope'.

## A note on node names

When node names are used in the API, unless explicitly specified otherwise, the node's alias in the [database](/docs/database.md) is used. This is the short dns name of the node's name as reported by `os.uname()`, so `node` instead of `node.domain.tld`.

## URL Format

By default dfapi servers run on port 9192. The format of the URL used is:

`https://node[:port].taild0af6d.ts.net/{module}/{endpoint}/{data}`

It's strongly suggested that POST is used for the request method. 

### Headers

* Authorization. The headers must include the pre-shared API (from server.ini) key must be sent as a bearer token, e.g., `Authorization: Bearer 42aa6022d87147cbb89a47d0d743e9b1` in the request headers.
* Content-Type must be `Content-Type: application/json`

Most API calls require a valid [ticket](/docs/ticket.md). The ticket's id must be passed to the API in the POST or GET data as `ticket_id`. 

Data to be passed to a module should be sent in the `data` key of the payload data.

## Response Envelope

The response envelope is formatted as below. Each request is assigned an ID, contains the modinfo property of the plugin module called, server information and request information. Timing of the request is included in the request information. The data property is where the specific module's response is stored.

This response envelope could be used for tracking of requests. The _id property makes it perfect for using as a MongoDB document.

<details>
<summary><b>Response Envelope Example</b></summary>

```json
{
    "_id": "00db9f2d-e61d-47d2-937b-7753c67a9d91",
    "status": "ok",
    "modinfo": "com.ducksfeet.dfapi.ping:v0.0.4",
    "server_info": {
        "api_server_name": "nodey.taild0af6d.ts.net",
        "api_server_addr": "100.108.212.120",
        "physical_node": "nxp.ducksfeet.com",
        "physical_address": "192.168.1.93"
    },
    "request": {
        "_id": "00db9f2d-e61d-47d2-937b-7753c67a9d91",
        "request_path": "ping",
        "request_time": 1775710377,
        "response_time": 0.002866506576538086,
        "client": {
            "ip": "100.65.200.27",
            "node": "nodex.taild0af6d.ts.net"
        }
    },
    "data": {}
```

</details>
