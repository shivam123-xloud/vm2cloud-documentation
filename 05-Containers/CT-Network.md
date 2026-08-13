# Container Network

---

## Overview

The **Network** tab configures a container's network interfaces — which bridge they attach to, how they get an address, and whether the firewall applies to them.

Containers differ from virtual machines here. A virtual machine's network devices live under **Hardware**; a container has its own **Network** tab. If you go looking under Hardware for a container, you will not find it — containers have no Hardware tab at all.

A container interface is a virtual NIC attached to a bridge on the host. The bridge determines which physical network the container can reach. See [Manage Linux Bridge](../03-Nodes/System/Network/Manage-Linux-Bridge.md).

---

## When to Use

Open the Network tab when you need to:

* Give a new container network access.
* Change a container's IP address.
* Move a container to a different bridge or VLAN.
* Add a second interface so a container reaches two networks.
* Enable firewall filtering on an interface.
* Change from DHCP to a static address, or back.
* Troubleshoot a container with no connectivity.

---

## Prerequisites

Before configuring container networking, ensure that:

* You have permission to modify the container.
* The bridge you intend to attach to exists on the node. See [Network Overview](../03-Nodes/System/Network/Network-Overview.md).
* You know the addressing method — DHCP or static.
* For a static address, you have the address, prefix, and gateway.
* For a VLAN, you know the tag and that the bridge is VLAN aware.
* You know whether a restart is acceptable — some changes need one.

---

# Procedure

## Step 1: Open the Network Tab

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select the container.
4. Click **Network**.

The container's interfaces are listed.

---

### Screenshot 1

**Container Network Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A container → Network, showing the interface list with the **Add**,
> **Edit**, and **Remove** buttons visible.

---

## Step 2: Add an Interface

1. Click **Add**.
2. Configure the fields described below.
3. Click **Add**.

---

### Screenshot 2

**Add Network Device Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add network device dialog for a container, empty, showing every field
> including Name, Bridge, VLAN Tag, Firewall, MAC address, and the IPv4 and IPv6
> configuration sections.

---

## Step 3: Set the Interface Name and Bridge

1. In **Name**, enter the interface name as it will appear inside the container, typically `eth0` for the first interface.
2. In **Bridge**, select the host bridge to attach to. This determines which network the container reaches.
3. If the network is tagged, enter the **VLAN Tag**.

The bridge is the decision that matters most. A container on the wrong bridge is on the wrong network, and no amount of correct addressing will fix that.

---

## Step 4: Configure IPv4

Choose the addressing method:

**DHCP** — the container requests an address from the network.

1. Select **DHCP** for IPv4.

**Static** — you specify the address.

1. Select **Static**.
2. In **IPv4/CIDR**, enter the address with its prefix, for example `192.168.1.50/24`.
3. In **Gateway (IPv4)**, enter the gateway address.

> **Warning:** Enter the address **with its prefix**. An address without a correct prefix, or with no gateway, leaves the container unable to reach anything beyond its local segment. This is the single most common container networking mistake.

---

### Screenshot 3

**Static IPv4 Configuration**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add or Edit network device dialog with Static selected and the
> IPv4/CIDR and Gateway fields filled in.

---

## Step 5: Configure IPv6

Configure IPv6 the same way if the network uses it:

* **DHCP** — request an address over DHCPv6.
* **SLAAC** — derive an address from router advertisements.
* **Static** — specify the address and gateway.
* **None** — no IPv6 on this interface.

If the network does not use IPv6, leaving it unset is fine.

> **Verify:** Confirm the exact IPv6 method names available in the dialog.

---

## Step 6: Enable the Firewall Checkbox

If this container should be filtered, tick the **Firewall** checkbox on the interface.

This is one of the two things required for container filtering. The other is enabling the firewall on the container's Firewall → Options panel. Rules do nothing unless both are set — see [Container Firewall](CT-Firewall.md).

---

### Screenshot 4

**Firewall Checkbox on the Interface**

```text
[ Place Screenshot Here ]
```

> **Capture:** The container network device dialog with the Firewall checkbox visible
> and ticked.

---

## Step 7: Confirm and Verify

1. Review the configuration.
2. Click **Add**, or **OK** if editing.
3. Open the [Console](Container-Console.md).
4. Confirm the interface has the expected address.
5. Confirm the container can reach its gateway and the wider network.

Some changes apply immediately; others require a container restart. If the change has not taken effect, restart the container.

---

### Screenshot 5

**Interface Configured**

```text
[ Place Screenshot Here ]
```

> **Capture:** The container Network tab showing a configured interface with its bridge,
> address, and firewall state.

---

## Step 8: Edit or Remove an Interface

**To edit:**

1. Select the interface.
2. Click **Edit**.
3. Change the required fields.
4. Click **OK**.

**To remove:**

1. Select the interface.
2. Click **Remove**.
3. Confirm.

> **Warning:** Removing the interface a container is reached through cuts off its network access immediately. You will need the [Console](Container-Console.md) to reconfigure it, since console access does not depend on container networking.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Name** | Interface name inside the container, for example `eth0`. |
| **Bridge** | Host bridge the interface attaches to. Determines which network the container reaches. |
| **VLAN Tag** | VLAN ID, when the bridge is VLAN aware and the network is tagged. |
| **Firewall** | Whether firewall rules apply to this interface. Required for container filtering. |
| **MAC address** | Hardware address. Generated automatically; set manually only when something external depends on a fixed MAC. |
| **Rate limit** | Caps bandwidth for this interface. |
| **IPv4** | **DHCP** or **Static**. Static requires an address in CIDR form. |
| **Gateway (IPv4)** | Default gateway for IPv4. Required for anything beyond the local segment. |
| **IPv6** | **DHCP**, **SLAAC**, **Static**, or **None**. |
| **Gateway (IPv6)** | Default gateway for IPv6. |

> **Verify:** Capture the complete Add network device dialog and confirm the exact field
> labels and available options.

---

# Verification

Verify the following:

* The interface appears in the Network tab with the intended bridge.
* Inside the container, the interface exists and has the expected address.
* The container can reach its gateway.
* The container can reach the wider network and resolve names — see [CT DNS](CT-DNS.md).
* Other hosts can reach the container on the ports it serves.
* The VLAN tag is correct, if used.
* The firewall checkbox matches your filtering intent.
* Rate limiting behaves as expected, if configured.

Check from the console rather than assuming, since a networking mistake removes the access you would otherwise use to check.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| No network access at all | Check the bridge first. A container on the wrong bridge is on the wrong network. |
| Can reach the local network but nothing beyond | The gateway is missing or wrong, or the prefix is incorrect. |
| Address not applied after editing | Restart the container. Some changes need one. |
| Firewall rules have no effect | The Firewall checkbox on the interface is not ticked. See [Container Firewall](CT-Firewall.md). |
| VLAN traffic not passing | The bridge must be VLAN aware. Check the bridge configuration on the node. |
| No address from DHCP | Confirm a DHCP server serves that bridge, and that firewall filtering is not blocking DHCP. |
| Names do not resolve | Networking is working but DNS is not. See [CT DNS](CT-DNS.md). |
| Duplicate address on the network | Two containers were given the same static address, or a static address overlaps the DHCP range. |
| Cannot find the setting under Hardware | Containers configure networking on the **Network** tab. Only virtual machines use Hardware. |
| Container unreachable after a change | Use the [Console](Container-Console.md), which does not depend on container networking. |

---

# Best Practices

- Confirm the bridge before anything else. Most connectivity problems are a wrong bridge, not a wrong address.
- Always enter static addresses in CIDR form, with a gateway.
- Prefer DHCP with reservations over manually assigned static addresses where the network supports it — it keeps addressing in one place.
- Tick the Firewall checkbox at creation if the container will ever be filtered, so filtering is never half-configured.
- Verify from the console after changing networking.
- Keep a record of static addresses so two containers never collide.
- Use rate limits on containers that could otherwise saturate a shared link.
- Change networking during a maintenance window for anything in production.

---

# Related Documentation

- [CT DNS](CT-DNS.md)
- [Container Firewall](CT-Firewall.md)
- [Container Console](Container-Console.md)
- [Create Container](Create-Container.md)
- [Manage Container Resources](Manage-Container-Resources.md)
- [Container Troubleshooting](Container-Troubleshooting.md)
- [Network Overview](../03-Nodes/System/Network/Network-Overview.md)
- [Manage Linux Bridge](../03-Nodes/System/Network/Manage-Linux-Bridge.md)
- [Manage VLAN](../03-Nodes/System/Network/Manage-VLAN.md)

---

# Summary

The Network tab configures a container's interfaces: which host bridge they attach to, how they obtain an address, whether a VLAN tag applies, and whether the firewall filters them. Containers keep this on their own tab rather than under Hardware, which is where virtual machines put it.

The bridge is the setting that decides which network the container can reach, and a wrong bridge cannot be fixed by correcting the address. For static addressing, always supply the prefix and the gateway. And if the container will be filtered, tick the Firewall checkbox here — rules on the Firewall tab do nothing without it.
