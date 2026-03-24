# DucksFeet API

 "Look at a duck on the pond. On the surface, everything looks calm, 
 but beneath the water those little feet are churning like crazy." 
 - Jimmy McGinty (The Replacements)

 This is the DucksFeet core philosophy - be busy without looking busy. 

* [CLI Tool](#cli-tool)
* [Authentication](#Authentication)
* [Endpoints](#Endpoints)
* [Modules](#Modules)
* [API Users](#apiuser)
* [Ticket](#Ticket)
* [Services](#Services)
* [System Information](#System-Information)
* [Examples](#Examples)
    * [Example of Authentication with Cross Host Ticket Use](#Example-of-Authentication-with-Cross-Host-Ticket-Use)
    * [Example of Service output](#Example-of-Service-output)
        * [List](#List)
        * [Status](#Status)
    * [Example System Information Blob](#Example-System-Information-Blob)

The DucksFeet API is a distributed system management platform designed for secure, low-overhead monitoring and control. By utilizing a REST-like architecture served over a private Tailscale network, it provides a unified interface for interacting with diverse hosts—from Raspberry Pi sensors to Proxmox VMs—using a standardized ticketing system for secure, cross-host authorization.

The examples in this document use the [CLI Tool](#cli-tool) and [jq](https://jqlang.org) to process the JSON data the tool produces. [jq](https://jqlang.org) is a powerful command-line JSON processor that can be used in shells and shell scripting. 

## CLI Tool

The DucksFeet API CLI tool, dfapi, is a python module that provides command line access to the DucksFeet API ecosystem. 
The tool handles authentication and ticket acquisition to perform actions. The tickets are cached in the rc-file. 

### dfapi Usage and Configuration

usage: `dfapi.py [-h] [-d] [-p integer] [-r file.json] [-s] endpoint [args ...]`

Arguments

| Options | Usage |
|---------|-------|
|  -d, --debug|           Enable debug messages |
|  -p, --port integer |   Use port specified, default 9192 |
|  -r, --rc-file |   Load settings from file, default ~/.dfapi.settings.json |
|  -s, --stdin |          Read API data from stdin |
| -h, --help|            Show help message and exit |

Positional arguments:
*  endpoint: Endpoint to call
*  args: Arguments for the endpoint in JSON format

#### API Endpoint specification

graph TD
    ROOT["//HOST | @"] --> MODULE["/MODULE/"]
    MODULE --> TARGET["/TARGET/"]
    TARGET --> PARMS["/...PARMS"]

The rc-file is a JSON file in the form of

```json
{
  "credentials": "/home/user/etc/dfapi.json",
  "ticket": "ascii ticket id"
}
```

Note the tickets entry are cached ticket id's and should not be directly set by the user. The important key is the credentials entry that points to a credentials file. This file and the credentials file must be owned by the user and mode `600`. (`owner r/w`).

And the credentials file is in the format of

```json
{
  "user_id": "sysadm",
  "password": "sf56asecurePassword4224",
  "apikey": "wqwffsd33434346gdfhjjfjfjf3333"
}
```


## Authentication

Access control is managed via tickets. Tickets are a unique ID that lasts a server configurable timeout, the default is 600 seconds or 10 minutes. 

Tickets are bound to the requesting IP address. Tickets can have session data associated with the ticket. This data is expires at the same time the ticket does. 

To obtain a ticket, a user_id and password are supplied. This is used to determine the identify of the user and the the roles assigned to that user. These roles determine permission to do operations defined by the particular modules. 

When using the CLI tool or API these operations are performed automatically and cached. 


## Endpoints

DFApi uses consistent endpoint module and endpoints. Endpoints use a UNC style path to target a host. The host is looked up to get the FQDN of the host for constructing a URL that works with the SSL certificate of that host. 

The endpoints are formatted as 

```bash 
//host/module/[target][/qualifier/...]
```
## Modules

|module|Use|
|---------|---|
| [apiuser](#apiuser) | User control module |
| [services](#services) | Systemd tools|
| [systeminfo](#system-information)| System examination |
| [ticket](#ticket)| Access control tickets |

### APIUser
The apiuser module manages users for the DucksFeet API ecosystem. These users are used to access the API with given roles. See, also, [Authentication](#Authentication)

The endpoints for apiuser are: 
|Endpoint|Descrtiption|Roles Required|
|--------|------------|--------------|
| apiuser/list | List users | user_view |
| apiuser/*user*/new | Creates a new user from data | user_view, user_modify |
| apiuser/*user*/modify | Modifies a new user from data | user_view, user_modify |
| apiuser/*user*/delete | Deletes a new user | user_view, user_modify |
| apiuser/*user*/grant/*role* | Grant a role to a user | user_view, user_modify |
| apiuser/*user*/revoke/*role* | Revoke role from a user | user_view, user_modify |

### Services

The services module uses pre-defined roles that are defined in the user record and tied to the ticket. Operations must have either *service_read*  *service_control*. These endpoints provide direct access to some systemd functions and require granular control via RBAC and ticketing operations. 

The services endpoints are:

|Endpoint|Description| Role Required |
|----|----|----|
|services/list | List all services | service_read|
|services/*service*/status | Get Detailed service status| service_read |
|services/*service*/start | Start service | service_control |
|services/*service*/stop | Stop service | service_control |
|services/*service*/restart| Restart service | service_control |

Example of service control and response:

```bash
 dfapi //hp/services/dummy-target/start
{
    "action": "start",
    "service_name": "dummy-target",
    "command_status": true,
    "return_code": 0,
    "message": ""
}
```

The services/list endpoint will produce *lots* of data.


### System Information

This module provides detailed data about the target host. This comprehensive 'blob' (a JSON object) contains identification data on the host, the operating system, whether it's a virtual machine, cpu, network statistics, the count of packages that need to be upgraded, the cpu architecture, ram and disk sizes and usage.

This data can be used to monitor the state of a host and issue alerts when limits are not met or used to display detailed system information in dashboards. 

The endpoints are a subset of the full blob that can be returned. If data from multiple endpoints is needed it's better to request the full blob and index it, e.g.:

```bash
 dfapi //hp/systeminfo | jq '{
  "tailnet": (.net.tailscale0.tx_speed),
  "lan": (.net.eth0.tx_speed),
  "cpu": (.cpu.percent),
  "ram": (.ram.percent),
}'
{
  "tailnet": 7406,
  "lan": 10300,
  "cpu": 1.1,
  "ram": 50.8
}
```
Here we see the advantage of one request but extracting different data. If, on the other hand, you are just interested ram usage you could just get that data. E.g.:

```bash
 dfapi //hp/systeminfo/ram | jq '{"ram": (.percent)}'
{
  "ram": 51.2
}
```

This is module generates large amounts of data.

Endpoints:

|Endpoint|Description|
|-----------|-------------|
|systeminfo|Full systeminformation object|
|systeminfo/net| Network Devices|
|systeminfo/cpu| CPU Data|
|systeminfo/uptime| System Uptime|
|systeminfo/storage| Storage Information|
|systeminfo/upgrades| Upgradeable Packages|
|systeminfo/ram| Ram Information|
|systeminfo/info| General System Information|
/systeminfo/list|List available endpoints|

### Ticket

Each ticket target, except new, requires a ticket_id sent as a POST or GET parameter either in POST JSON data (herein referred to as 'data'). 

End points

|Target| Description |
|--------|------------|
|ticket/new     |Create new ticker with data: {"user_id": *userid*, "password": *password*}|
|ticket/delete| Delete the current ticket|
| ticket/validate | Validate the current ticket|
| ticket/set_session| Set session data in for current ticket with data {"session_data": *JSON object*}|
| ticket/get_session| Get session data for current ticket|

Example using the DFApi Tool:

```bash 
cat etc/dfapi.json|  dfapi -s //host/ticket/new
"Uda7fWcsIlk4UANcf249m4QGyYfyM1ViRQwhmdYyp-8"
```

Or with curl, you can see the raw data: 

```bash
 curl -Ss -d "$(cat etc/dfapi.json)" https://host:9192/ticket/new
{"status": "ok", "request_path": "new", "data": "K2Xz5xuiIe18ss8A8Z2QtZpOK3RwhnyGNKRXcdMvToc"}
```

This uses authentication data store in etc/dfapi.json to send to the host the ticket/new endpoint and returns the quoted ticket_id.

## Examples

#### Example of Authentication with Cross Host Ticket Use

Files:

* `~/etc/dfapi.json` - Contains credentials and secret key needed foor all transactions
* `~/etc/dfapi-key.json` - Contains just the secret key needed. 

```bash
# Using curl(1) to get a ticket from host a

curl -Ss -d "$(cat etc/dfapi.json)"  https://hosta.example.com:9192/ticket/new | jq -r '.data'
P_pu3VAQ4AwtfBoPTLGkph1WqMLkAllMfpynL45YbJM

# Using curl(1) to get system information from hostb with the ticket obtained from hosta 

curl -Ss -d "$(cat ~/etc/dfapi-key.json)" https://hostb.example.com:9192/systeminfo/info\?ticket_id\=P_pu3VAQ4AwtfBoPTLGkph1WqMLkAllMfpynL45YbJM
{"status": "ok", "request_path": "info", "data": {"virtual_machine": true, "machine": "x86_64", "sysname": "Linux", "version": "#1 SMP PREEMPT_DYNAMIC Debian 6.1.159-1 (2025-12-30)", "release": "6.1.0-42-amd64", "nodename": "hostb", "os-release": {"PRETTY_NAME": "Debian GNU/Linux 12 (bookworm)", "NAME": "Debian GNU/Linux", "VERSION_ID": 12, "VERSION": "12 (bookworm)", "VERSION_CODENAME": "bookworm", "ID": "debian", "HOME_URL": "https://www.debian.org/", "SUPPORT_URL": "https://www.debian.org/support", "BUG_REPORT_URL": "https://bugs.debian.org/"}}}

```

#### Example of Service output

##### List
```bash
dfapi //hp/services/list [
    {
        "unit": "boot.automount",
        "load": "not-found",
        "active": "inactive",
        "sub": "dead",
        "description": "boot.automount"
    },
    {
        "unit": "Extra.automount",
        "load": "loaded",
        "active": "active",
        "sub": "running",
        "description": "Extra.automount"
    },
    {
        "unit": "net-Duckie.automount",
        "load": "loaded",
        "active": "active",
        "sub": "waiting",
        "description": "net-Duckie.automount"
    },
    {
        "unit": "proc-sys-fs-binfmt_misc.automount",
        "load": "loaded",
        "active": "active",
        "sub": "running",
        "description": "Arbitrary Executable File Formats File System Automount Point"
    },
    {
        "unit": "dev-cdrom.device",
        "load": "loaded",
        "active": "active",
        "sub": "plugged",
        "description": "QEMU_DVD-ROM d-live_12.11.0_xf_amd64"
    },
    {
        "unit": "dev-disk-by\\x2ddiskseq-1.device",
        "load": "loaded",
        "active": "active",
        "sub": "plugged",
        "description": "/dev/disk/by-diskseq/1"
    },
    {
        "unit": "dev-disk-by\\x2ddiskseq-2.device",
        "load": "loaded",
        "active": "active",
        "sub": "plugged",
        "description": "/dev/disk/by-diskseq/2"
    },
    ...
``` 

##### Status
service/*service*/status also produces a large amount of data:

```bash
 dfapi //hp/services/rapi/status
 {
    "Type": "simple",
    "ExitType": "main",
    "Restart": "always",
    "PIDFile": null,
    "NotifyAccess": "none",
    "RestartUSec": "1s",
    "TimeoutStartUSec": "1min 30s",
    "TimeoutStopUSec": "1min 30s",
    "TimeoutAbortUSec": "1min 30s",
    "TimeoutStartFailureMode": "terminate",
    "TimeoutStopFailureMode": "terminate",
    "RuntimeMaxUSec": "infinity",
    "RuntimeRandomizedExtraUSec": "0",
    "WatchdogUSec": "0",
    "WatchdogTimestamp": null,
    "WatchdogTimestampMonotonic": "0",
    "RootDirectoryStartOnly": "no",
    "RemainAfterExit": "no",
    "GuessMainPID": "yes",
    "RestartPreventExitStatus": null,
    "RestartForceExitStatus": null,
    "SuccessExitStatus": null,
    "MainPID": "2110257",
    "ControlPID": "0",
    "BusName": null,
    "FileDescriptorStoreMax": "0",
    "NFileDescriptorStore": "0",
    "StatusText": null,
    "StatusErrno": "0",
    "Result": "success",
    "ReloadResult": "success",
    "CleanResult": "success",
    "USBFunctionDescriptors": null,
    "USBFunctionStrings": null,
    "UID": "[not set]",
    "GID": "[not set]",
    "NRestarts": "0",
    "OOMPolicy": "stop",
    "ExecMainStartTimestamp": "Sun 2026-03-01 22:19:29 PST",
    "ExecMainStartTimestampMonotonic": "2600527272441",
    "ExecMainExitTimestamp": null,
    "ExecMainExitTimestampMonotonic": "0",
    "ExecMainPID": "2110257",
    "ExecMainCode": "0",
    "ExecMainStatus": "0",
    "ExecStart": "{ path=/opt/rapi/rapi-server ; argv[]=/opt/rapi/rapi-server ; ignore_errors=no ; start_time=[Sun 2026-03-01 22:19:29 PST] ; stop_time=[n/a] ; pid=2110257 ; code=(null) ; status=0/0 }",
    "ExecStartEx": "{ path=/opt/rapi/rapi-server ; argv[]=/opt/rapi/rapi-server ; flags= ; start_time=[Sun 2026-03-01 22:19:29 PST] ; stop_time=[n/a] ; pid=2110257 ; code=(null) ; status=0/0 }",
    "Slice": "system.slice",
    "ControlGroup": "/system.slice/rapi.service",
    ...
```  

#### Example System Information Blob

```json
 dfapi //hp/systeminfo
{
    "modinfo": "com.ducksfeet.rapi.sysinfo:v1.2",
    "net": {
        "lo": {
            "iface": "lo",
            "address": "127.0.0.1/8",
            "tx_speed": 168,
            "rx_speed": 168,
            "statistics": {
                "bytes_sent": 176388442,
                "bytes_recv": 176388442,
                "packets_sent": 1356088,
                "packets_recv": 1356088,
                "errin": 0,
                "errout": 0,
                "dropin": 0,
                "dropout": 0
            }
        },
        "eth0": {
            "iface": "eth0",
            "address": "192.168.1.77/24",
            "tx_speed": 9828,
            "rx_speed": 6700,
            "statistics": {
                "bytes_sent": 9803380517,
                "bytes_recv": 13201741464,
                "packets_sent": 28582124,
                "packets_recv": 51681967,
                "errin": 0,
                "errout": 0,
                "dropin": 4983,
                "dropout": 0
            }
        },
        "tailscale0": {
            "iface": "tailscale0",
            "address": "100.101.222.53/32",
            "tx_speed": 7102,
            "rx_speed": 4715,
            "statistics": {
                "bytes_sent": 5555337246,
                "bytes_recv": 3830650216,
                "packets_sent": 13941564,
                "packets_recv": 19176409,
                "errin": 0,
                "errout": 0,
                "dropin": 0,
                "dropout": 0
            }
        }
    },
    "cpu": {
        "cpu_temperature": 0,
        "cpu_model": "QEMU Virtual CPU version 2.5+",
        "cpu_count": 8,
        "cpu_freq": [
            2904.0760000000005,
            0.0,
            0.0
        ],
        "loadavg": [
            0.0927734375,
            0.0615234375,
            0.0556640625
        ],
        "percent": 4.3,
        "times": [
            68221.33,
            19.71,
            17472.42,
            21194563.22,
            3162.17,
            0.0,
            4195.89,
            729.76,
            0.0,
            0.0
        ],
        "stats": [
            1430813925,
            982359595,
            666146769,
            0
        ]
    },
    "uptime": {
        "pretty": "up 4 weeks, 2 days, 20 hours, 2 minutes",
        "seconds": 2664160.67
    },
    "storage": [
        {
            "friendly_dev": "EFIBOOT",
            "dev": "LABEL=EFIBOOT",
            "mount": "/boot/efi",
            "fstype": "vfat",
            "total": 535801856,
            "free": 523550720,
            "used": 12251136,
            "percent": 2
        },
        {
            "friendly_dev": "PNY250",
            "dev": "LABEL=PNY250",
            "mount": "/",
            "fstype": "xfs",
            "total": 239401926656,
            "free": 188825722880,
            "used": 50576203776,
            "percent": 21
        },
        {
            "friendly_dev": "PNY500",
            "dev": "LABEL=PNY500",
            "mount": "/home",
            "fstype": "xfs",
            "total": 499325988864,
            "free": 440325464064,
            "used": 59000524800,
            "percent": 11
        }
    ],
    "upgrades": {
        "upgradable_packages": 100
    },
    "ram": {
        "total": 8303169536,
        "free": 1170882560,
        "used": 4246175744,
        "cached": 3252510720,
        "percent": 51.1,
        "active": 1470500864,
        "buffers": 2695168,
        "inactive": 4285255680,
        "shared": 49778688,
        "slab": 788754432
    },
    "info": {
        "virtual_machine": true,
        "machine": "x86_64",
        "sysname": "Linux",
        "version": "#1 SMP PREEMPT_DYNAMIC Debian 6.1.159-1 (2025-12-30)",
        "release": "6.1.0-42-amd64",
        "nodename": "hp",
        "os-release": {
            "PRETTY_NAME": "Debian GNU/Linux 12 (bookworm)",
            "NAME": "Debian GNU/Linux",
            "VERSION_ID": 12,
            "VERSION": "12 (bookworm)",
            "VERSION_CODENAME": "bookworm",
            "ID": "debian",
            "HOME_URL": "https://www.debian.org/",
            "SUPPORT_URL": "https://www.debian.org/support",
            "BUG_REPORT_URL": "https://bugs.debian.org/"
        }
    },
    "module_time": 1772496002
}

```
