# PVE

The pve module is a very simple module. Effectively, it's a proxy to the Proxmox API by way of the [papi](/lib/papi.py) module.

It expects a file, permissions protected, in `/etc/pvecreds.json`.

The pve module, on initialization, creates an `aiohttp.ClientSession` objet which is passed along to the papi module to perform requests. This allows for connection pooling across multiple, simultaneous, requests with lower resource usage. 

## Endpoint Format

* Usage: `pve/{method}/{proxmox api endpoint}`
* Role required: pve
* Data required: Undefined

Proxmox uses are more stringent policy with request methods. The method specified will be used to make the Proxmox request.

Any data needed to be passed the Proxmox API will be sent along the proxy from in the request payload data property.

The response value is whatever the proxmox api returns and the data is the same data.
