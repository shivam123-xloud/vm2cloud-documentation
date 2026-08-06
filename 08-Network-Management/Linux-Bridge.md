# Linux Bridge

---

## Overview

A Linux Bridge is a software-based network switch that connects virtual machines and containers to the physical network. It forwards network traffic between physical network interfaces and virtual network interfaces, allowing workloads to communicate with each other and with external networks.

Linux Bridge is the default networking method used by VM2Cloud and is suitable for most virtualization environments.

---

## When to Use

Use a Linux Bridge when you need to:

- Connect virtual machines to a physical network.
- Connect containers to a physical network.
- Provide network connectivity to workloads.
- Configure management or production networks.
- Support VLAN-aware networking.

---

## Prerequisites

Before creating or modifying a Linux Bridge, ensure that:

- A physical network interface is available.
- The required IP addressing information is available.
- You have permission to manage network settings.
- You understand the impact of network changes on running workloads.

> **Warning:** Incorrect network configuration may temporarily disconnect the node from the network.

---

# Access Linux Bridge Configuration

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Click **Network**.

The Network page displays all configured network interfaces.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

# Linux Bridge Information

A Linux Bridge typically contains the following information:

- Bridge Name
- Interface Type
- Bridge Ports
- IP Address
- CIDR
- Gateway
- VLAN Aware Status
- Autostart Status

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Common Linux Bridge Configuration

A Linux Bridge may be configured using the following options.

| Option | Description |
|---------|-------------|
| Bridge Name | Name of the Linux Bridge (for example, `vmbr0`). |
| Bridge Ports | Physical network interface attached to the bridge. |
| IPv4/CIDR | IPv4 address assigned to the bridge. |
| IPv4 Gateway | Default gateway for the bridge. |
| IPv6/CIDR | IPv6 address assigned to the bridge (optional). |
| IPv6 Gateway | Default IPv6 gateway (optional). |
| VLAN Aware | Enables VLAN tagging on the bridge. |
| Autostart | Starts the bridge automatically during node boot. |

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Where Linux Bridges Are Used

Linux Bridges are commonly used for:

- Node management network
- Virtual machine networking
- Container networking
- Storage network
- Migration network
- VLAN-based networks

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The Linux Bridge appears in the Network page.
- The bridge status is active.
- The correct physical interface is attached.
- The configured IP address is correct.
- Virtual machines and containers connected to the bridge have network connectivity.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Bridge is down | Verify that the physical interface is connected and active. |
| Virtual machines cannot access the network | Ensure that the virtual NIC is connected to the correct Linux Bridge. |
| Incorrect IP configuration | Verify the IP address, subnet mask, and gateway settings. |
| VLAN traffic is not working | Confirm that VLAN Aware is enabled and that the switch port is configured correctly. |
| Network connectivity is lost after changes | Review the bridge configuration and restore the previous settings if necessary. |

---

# Summary

A Linux Bridge acts as a virtual network switch that connects virtual machines, containers, and physical network interfaces. It is the default networking method in VM2Cloud and provides a flexible and efficient way to enable communication between workloads and external networks.
