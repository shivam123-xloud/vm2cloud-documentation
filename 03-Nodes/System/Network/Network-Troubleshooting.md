# Network Troubleshooting

---

## Overview

This guide provides solutions for common network-related issues that administrators may encounter while managing networking in VM2Cloud. It covers problems related to Linux Bridges, Bonds, VLANs, physical interfaces, DNS, gateways, and network connectivity.

Use this guide to identify common issues, understand their possible causes, and apply the appropriate resolution.

---

## When to Use

Refer to this guide when:

* The node cannot access the network.
* A Linux Bridge is not working.
* A Bond interface is down.
* A VLAN has no connectivity.
* DNS resolution fails.
* The default gateway is unreachable.
* Virtual machines cannot communicate with the network.
* Network configuration changes fail.

---

# Issue 1: Node Has No Network Connectivity

### Possible Causes

* Physical network cable is disconnected.
* Incorrect IP address or subnet.
* Invalid gateway configuration.
* Switch port is unavailable.

### Resolution

* Verify that the network cable is connected.
* Check the configured IP address and subnet mask.
* Confirm that the default gateway is correct.
* Verify that the switch port is active.

---

# Issue 2: Linux Bridge Is Not Working

### Possible Causes

* Incorrect bridge configuration.
* Invalid bridge port.
* Bridge configuration has not been applied.
* Physical interface is down.

### Resolution

* Verify the bridge configuration.
* Confirm that the correct physical interface is attached.
* Apply the pending network configuration.
* Ensure the physical interface is operational.

---

# Issue 3: Bond Interface Is Down

### Possible Causes

* One or more member interfaces are disconnected.
* Incorrect bonding mode.
* Switch configuration does not match the selected bonding mode.

### Resolution

* Verify that all member interfaces are connected.
* Confirm the selected bonding mode.
* For LACP (802.3ad), verify that the switch is configured correctly.
* Apply the updated network configuration if changes were made.

---

# Issue 4: VLAN Has No Connectivity

### Possible Causes

* Incorrect VLAN ID.
* Parent interface is unavailable.
* VLAN is not allowed on the connected switch.

### Resolution

* Verify the VLAN ID.
* Confirm that the parent interface is active.
* Check the switch configuration to ensure the VLAN is permitted.

---

# Issue 5: Virtual Machines Cannot Access the Network

### Possible Causes

* Virtual machine is connected to the wrong bridge.
* Bridge is not connected to the physical network.
* VLAN configuration is incorrect.

### Resolution

* Verify the virtual machine's network interface configuration.
* Confirm that the selected Linux Bridge is operational.
* Verify VLAN settings if VLANs are in use.

---

# Issue 6: DNS Resolution Fails

### Possible Causes

* Incorrect DNS server configuration.
* DNS server is unavailable.
* Network connectivity issue.

### Resolution

* Verify the configured DNS server addresses.
* Confirm that the DNS server is reachable.
* Test network connectivity before troubleshooting DNS.

---

# Issue 7: Default Gateway Is Unreachable

### Possible Causes

* Incorrect gateway address.
* Gateway device is offline.
* Incorrect subnet configuration.

### Resolution

* Verify the configured default gateway.
* Confirm that the gateway device is operational.
* Ensure that the gateway belongs to the same network as the interface.

---

# Issue 8: Unable to Apply Network Configuration

### Possible Causes

* Invalid network configuration.
* Interface conflict.
* Duplicate IP address.
* Configuration validation failed.

### Resolution

* Review the pending network configuration.
* Correct any invalid settings.
* Verify that IP addresses are unique.
* Apply the configuration again after resolving the issue.

---

# Common Diagnostic Commands

The following commands can help diagnose network-related issues.

---

## Display Network Interfaces

```bash id="netcmd01"
ip addr
```

Displays all configured network interfaces and their IP addresses.

---

## Display Routing Table

```bash id="netcmd02"
ip route
```

Displays the current routing table, including the default gateway.

---

## Test Network Connectivity

```bash id="netcmd03"
ping <IP_Address>
```

Tests connectivity to another device on the network.

Example:

```bash id="netcmd04"
ping 192.168.1.1
```

---

## Test DNS Resolution

```bash id="netcmd05"
ping google.com
```

Verifies that DNS name resolution is working correctly.

---

## Display Network Interface Status

```bash id="netcmd06"
ip link
```

Displays the operational status of all network interfaces.

---

## Verify Network Configuration

```bash id="netcmd07"
cat /etc/network/interfaces
```

Displays the current network configuration configured on the node.

---

## Restart Networking Service

> **Note:** Restarting networking may temporarily disconnect the node from the network.

```bash id="netcmd08"
ifreload -a
```

Applies the network configuration from the command line.

---

# Verification

After resolving the issue, verify the following:

* The node is reachable over the network.
* Linux Bridges are operational.
* Bond interfaces are active.
* VLAN interfaces are functioning correctly.
* DNS resolution is working.
* The default gateway is reachable.
* Virtual machines and containers have network connectivity.

---

# Summary

Most networking issues in VM2Cloud are caused by incorrect interface configuration, bridge settings, VLAN configuration, bonding configuration, or gateway and DNS settings. Reviewing the network configuration, validating physical connectivity, and using the available diagnostic commands will resolve the majority of network-related problems.
