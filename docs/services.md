# Services

The services module does some basic service control using systemd. Users of this module must have `service_read` or `service_control` roles to perform the functions below, given the risk potential for this module.

These endpoints can produce a lot of data and because they rely on systemd to do work may take longer than other endpoints and modules.

## Endpoint Format

* [list](#list) - List endpoints.
* [status](#status) - Get service statius.
* [start](#start) - Start a service.
* [stop](#stop) - Stop a service.
* [restart](#restart) - Restart a service.

### list

* Usage: `services/list`
* Role required: service_read

### status

* Usage: `services/{service}/status`
* Role required: service_read

### start

* Usage: `services/{service}/start`
* Role required: service_control

### stop

* Usage: `services/{service}/stop`
* Role required: service_control

### restart

* Usage: `services/{service}/restart`
* Role required: service_control

<details>
<summary><b>Example service control response</b></summary>

```json
{
    "action": "start",
    "service_name": "dummy-target",
    "command_status": true,
    "return_code": 0,
    "message": ""
}
```

</details>
