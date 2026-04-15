# CLI Tool

The DucksFeet API CLI tool, dfapi, is a python module that provides command line access to the DucksFeet API ecosystem.

The tool handles authentication and ticket acquisition to perform actions. The tickets are cached in the config file.

## dfapi Usage and Configuration

usage: `dfapi.py [-h] [-c file.json ] [-d] [-p integer] [-r file.json] [-s] endpoint [args ...]`

<details>
<summary><b>Flags Detail</b></summary>

|Flag|Usage|
|----|----|
| -c, --config file.json | Load configuration from file.json, default .dfapi.config.json. |
|  -d, --debug           | Enable debug messages. |
|  -e, --envelope        | show response envelope. |
|  -p, --port integer    | Use port specified, default 9192. |
|  -r, --read            | Read API data from file.json. |
|  -s, --stdin           | Read API data from stdin. |
|  -t, --time            | Show api call time. |
|  -h, --help            | Show help message and exit. |

Positional arguments:

* endpoint: Endpoint to call
* args: Arguments for the endpoint in JSON format

</details>


### API Endpoint specification

[//*node*[:*port*]|*@*]/*module*/*target*/*param1*/*param2*/*etc*

* node - the node on which to call the api or *@* for all nodes.
* port - the port to use (see usage below)
* module - this is the api modules ot use 
* target - optional target of the module. 
* param... - optional parameters 

Typically, the port need not be used unless testing on a port other than the default of port 9192. 
The unc-style path is first normalized from: 
`//node/module/parm1/param2` to `//node.taild0af6d.ts.net:9192/module/param1/param2` and from that path we get the api url of `https://bigwhoop.taild0af6d.ts.net:9192/module/param1/param2`. This final URL is what is actually requested for service on.

<details>
<summary><b>Config File Format</b></summary>

The config file is a JSON file in the form of

```json
{
  "credentials": "/home/user/etc/dfapi.json",
  "ticket": "ascii ticket id"
}
```

Note the ticket entry is the cached last known ticket id and should not be directly set by the user. The important key is the credentials entry that points to a credentials file. This file and the credentials file must be owned by the user and mode `600`. (`owner r/w`).

And the credentials file is in the format of

```json
{
  "user_id": "sysadm",
  "password": "sf56asecurePassword4224",
  "apikey": "wqwffsd33434346gdfhjjfjfjf3333"
}
```
</details>
