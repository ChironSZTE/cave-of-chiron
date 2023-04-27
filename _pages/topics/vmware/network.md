---
title: "VMware"
tags: vmware
sidebar:
nav: "vmware"

---
## VMware network settings:



| Network Connection Settings Setting       | Description                                                                                                                                                                                                                               |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Use **bridged** networking                | Configure a bridged network connection for the virtual machine. With bridged networking, the virtual machine has direct access to an external Ethernet network. The virtual machine must have its own IP address on the external network. |
| Use network address translation **(NAT)** | Select NAT if you do not have a separate IP address for the virtual machine, but you want to be able to connect to the Internet.                                                                                                          |
| Use **host-only** networking              | With host-only networking, the virtual machine can communicate only with the host system and other virtual machines in the host-only network. Select host-only networking to set up an isolated virtual network.                          |
| **Do not use** a network connection       | Do not configure a network connection for the virtual machine.                                                                                                                                                                                                                                          |

[docs.vmware.com](https://docs.vmware.com/en/VMware-Workstation-Pro/16.0/com.vmware.ws.using.doc/GUID-3B504F2F-7A0B-415F-AE01-62363A95D052.html)

![21.png](/images/21.png)


