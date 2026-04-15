# System Information

The systeminfo module provides detailed data about the target node. This comprehensive 'blob' (a JSON object) contains identification data on the host, the operating system, whether it's a virtual machine, cpu, network statistics, the count of packages that need to be upgraded, the cpu architecture, ram and disk sizes and usage.

This data can be used to monitor the state of a node and issue alerts when limits are not met or used to display detailed system information in dashboards. Or aggregate into a fleet-wide dashboard.

Without an additional endpoint the system information module returns a full system information blob.

## Endpoint format

* [cpu](#cpu) - CPU Data
* [uptime](#uptime) - System Uptime
* [info](#info) - General System Information
* [net](#net) - Network Devices
* [ram](#ram) - Ram Information
* [storage](#storage) - Storage Information
* [swap](#swap) - Swap information
* [hardware](#hardware) - Hardware information
* [upgrades](#upgrades) - Upgradeable Packages

### cpu

* Usage: `systeminfo/cpu`
* Roles required: none.

Return CPU information.

### hardware

* Usage: `systeminfo/hardware`
* Roles required: none.

Return hardware details.

### info

* Usage: `systeminfo/info`
* Roles required: none.

Return general system information.

### net

* Usage: `systeminfo/net`
* Roles required: none.

Return interface information.

### ram

* Usage: `systeminfo/ram`
* Roles required: none.

Return RAM informaiton.

### storage

* Usage: `systeminfo/storage`
* Roles required: none 

Return storage informaiton on storage devices defiend in fstab.

### swap

* Usage: `systeminfo/swap`
* Roles required: none.

Return the state of the system swap.

### upgrades

* Usage: `systeminfo/upgrades`
* Roles required: none.

Return return the count of packages requiring upgrades on the target node.

### uptime

* Usage: `systeminfo/uptime`
* Roles required: none.

Return the system uptime.
