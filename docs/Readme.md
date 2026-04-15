# DucksFeet API

<img src="/docs/ducksfeet-left.svg" height="100" width="100"  style="float: left; margin-right: 15px;">


"*Look at a duck on the pond. On the surface, everything looks calm, 
 but beneath the water those little feet are churning like crazy.*" - Jimmy McGinty "The Replacements"

 The core philosophy of the DuckFeet APi is for each module to do one thing and do it well while not impacting the system's performance. Keeping the pond calm, while the 'feet' below are paddling like crazy.

The DucksFeet API is a distributed system management platform designed for secure, low-overhead monitoring and control. By utilizing a REST-like architecture served over a private Tailscale network, it provides a unified interface for interacting with diverse hosts—from Raspberry Pi sensors to Proxmox VMs—using a standardized ticketing system for secure, cross-host authorization. The goal design is to provide a rich set of system and api management tools to centralize observation and control of a set of nodes.

Because of the diverse nature of service modules, it's important to check the  path and argument format for each module.

## Table of Contents

### Architectural Documents

* [Fundamentals of making api calls](/docs/api.md).
* [Specification of CLI and Endpoints](/docs/cli.md).
* [API User Management](/docs/apiuser.md)
* [Plugin Architecture](/docs/architecture.md).
* [Security Architecture](/docs/Security.md).

### Modules

* [jobs](/docs/jobs.md) - Long running jobs interface.
* [ping](/docs/ping.md) - Basic 'hello? are you there?'
* [procps](/docs/procps.md) - Process control related functions.
* [pve](/docs/pve.md) - Proxmox API proxy.
* [services](/docs/services.md) - Systemd tools.
* [systeminfo](/docs/systeminfo.md) - System observation.
* [ticket](/docs/ticket.md) - Access control tickets.

### Other Items

* [About](/docs/about.md) - About the author.
* [Gratitude](/docs/gratitude.md) - Expressing my gratitude.

## Installation

Please read the [installation](/docs/install.md) section for installation instructions and system requirements.
