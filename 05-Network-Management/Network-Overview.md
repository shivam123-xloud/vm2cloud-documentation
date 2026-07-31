# Network Overview

---

## Overview

Networking is a core component of VM2Cloud. It provides communication between physical servers, virtual machines, containers, storage systems, and external networks. Every virtual machine and container relies on a properly configured network to communicate with other systems.

VM2Cloud uses Linux networking technologies to provide flexible and reliable network connectivity for virtualized workloads.

---

## When to Use

Use the Network page to:

* View network interfaces.
* Configure Linux Bridges.
* Configure Bond interfaces.
* Create VLANs.
* Configure DNS and Gateway settings.
* Review existing network configuration.

---

## Prerequisites

Before managing network settings, ensure that:

* You have administrator privileges.
* The node is online.
* Physical network interfaces are connected.
* You understand the network configuration before making changes.

> **Important:** Incorrect network configuration may temporarily disconnect the node from the management interface.

---

# Network Configuration Page

To manage networking:

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Click **System**.
4. Select **Network**.

The Network page displays all network interfaces configured on the selected node.

---

### Screenshot 1

```text id="net01"
[ Place Screenshot Here ]
```

---

# Network Interfaces

The Network page displays all interfaces available on the selected node.

Depending on the server configuration, you may see:

* Physical Interfaces
* Linux Bridges
* Bond Interfaces
* VLAN Interfaces

Each interface displays information such as:

* Interface Name
* Type
* Active Status
* IP Address
* CIDR
* Gateway (if configured)
* Comments

---

### Screenshot 2

```text id="net02"
[ Place Screenshot Here ]
```

---

# Linux Bridge

A Linux Bridge acts as a virtual switch.

Virtual machines and containers connect to a Linux Bridge to communicate with the physical network or with other virtual machines.

A bridge is commonly attached to one or more physical network interfaces.

Typical bridge names include:

* vmbr0
* vmbr1
* vmbr10

---

### Screenshot 3

```text id="net03"
[ Place Screenshot Here ]
```

---

# Bond Interfaces

A Bond combines two or more physical network interfaces into a single logical interface.

Bonding is commonly used to provide:

* Network redundancy
* Increased availability
* Higher bandwidth (depending on the bonding mode)

Typical bond names include:

* bond0
* bond1

---

### Screenshot 4

```text id="net04"
[ Place Screenshot Here ]
```

---

# VLAN Interfaces

A VLAN (Virtual Local Area Network) logically separates network traffic over the same physical connection.

A VLAN interface is created using a parent interface or bond and a VLAN ID.

Example:

* bond0.100
* vmbr0.200

VLANs help isolate workloads and organize network traffic.

---

### Screenshot 5

```text id="net05"
[ Place Screenshot Here ]
```

---

# DNS Configuration

The Network page displays the DNS configuration used by the node.

DNS servers are responsible for resolving hostnames into IP addresses.

A correctly configured DNS server ensures reliable communication with internal and external services.

---

### Screenshot 6

```text id="net06"
[ Place Screenshot Here ]
```

---

# Gateway Configuration

The default gateway defines the route used for traffic destined outside the local network.

The configured gateway should be reachable from the management interface to ensure uninterrupted connectivity.

---

### Screenshot 7

```text id="net07"
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The required network interfaces are displayed.
* Linux Bridges are configured correctly.
* Bond interfaces show the expected member interfaces.
* VLAN interfaces display the correct VLAN ID.
* DNS and Gateway settings match the network design.

---

# Common Issues

| Issue                | Resolution                                                         |
| -------------------- | ------------------------------------------------------------------ |
| Interface is down    | Verify the physical cable and switch connection.                   |
| Incorrect IP address | Review the interface configuration and subnet settings.            |
| Missing bridge       | Confirm that the bridge has been created and applied successfully. |
| VLAN not displayed   | Verify the VLAN ID and parent interface configuration.             |
| Gateway unreachable  | Confirm that the configured gateway is reachable from the node.    |

---

# Summary

The Network page provides a centralized view of all network interfaces configured on a VM2Cloud node. From this page, administrators can review physical interfaces, Linux Bridges, Bonds, VLANs, DNS settings, and gateway configuration before performing network management tasks.
