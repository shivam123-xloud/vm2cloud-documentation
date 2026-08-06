# Network Overview

---

## Overview

Networking is a fundamental component of VM2Cloud that enables communication between nodes, virtual machines, containers, storage, and external networks. Network configuration determines how workloads connect to each other and to networks outside the virtualization environment.

VM2Cloud provides network management through software-defined network interfaces such as Linux Bridges and Network Bonds. These interfaces allow administrators to connect virtual machines and containers to physical networks while providing flexibility, redundancy, and improved performance.

---

## When to Use

Use Network Management to:

- Connect virtual machines and containers to a network.
- Configure network bridges.
- Aggregate multiple physical interfaces.
- Improve network redundancy.
- Increase network throughput.
- Configure VLAN-aware networking.
- Manage network connectivity for workloads.

---

## Prerequisites

Before configuring networking, ensure that:

- A VM2Cloud node is available.
- At least one physical network interface is connected.
- Required IP addressing information is available.
- You have permission to manage network settings.

---

# Access Network Management

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Click **Network**.

The Network page displays all network interfaces configured on the selected node.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

# Network Components

The Network page may display one or more of the following interface types.

| Network Component | Description |
|-------------------|-------------|
| Linux Bridge | Connects virtual machines and containers to the physical network. |
| Network Bond | Combines multiple physical interfaces for redundancy or increased bandwidth. |
| Physical Interface | Represents a physical network adapter installed in the node. |
| VLAN Interface | Provides communication over a specific VLAN. |
| Loopback Interface | Internal interface used by the operating system. |

> **Note:** The available interface types depend on your VM2Cloud deployment and hardware configuration.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Common Network Information

The Network page displays information such as:

- Interface Name
- Interface Type
- Active Status
- IP Address
- CIDR
- Gateway
- Bridge Ports
- Autostart Status

This information helps administrators monitor and manage network connectivity.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Available Actions

Depending on the selected interface, available actions may include:

- Create
- Edit
- Remove
- Reload Network Configuration

These actions allow administrators to manage the network configuration without manually editing configuration files.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The required network interfaces are displayed.
- Physical interfaces are detected correctly.
- Network bridges are available.
- IP addresses are configured correctly.
- Network status is displayed without errors.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Network interface is missing | Verify that the physical network adapter is connected and detected by the operating system. |
| Interface is down | Check the physical network connection and verify the interface configuration. |
| Incorrect IP configuration | Review the IP address, subnet mask, and gateway settings. |
| Network changes are not applied | Apply or reload the network configuration after making changes. |
| Virtual machines have no network connectivity | Verify that they are connected to the correct Linux Bridge and that the bridge is functioning correctly. |

---

# Summary

The Network page provides centralized management of all network interfaces configured on a VM2Cloud node. Administrators can monitor interface status, configure bridges and bonds, and manage connectivity for virtual machines and containers from a single location.
