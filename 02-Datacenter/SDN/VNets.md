# SDN VNets

---

## Overview

A **VNet** is the virtual network guests actually attach to. It appears on each node as a bridge, and once created and applied, it is selectable when configuring a guest's network interface exactly like any other bridge.

VNets live inside a [zone](Zones.md), which determines how their traffic reaches other nodes. Guests on the same VNet can reach each other; guests on different VNets cannot, unless routing is configured.

Optionally a VNet carries one or more **subnets** — an IP range with a gateway, and optional address management.

For how the layers fit together, see [SDN Overview](SDN-Overview.md).

---

## When to Use

Create a VNet when you need to:

* Give guests a network defined centrally rather than per node.
* Isolate a tenant, project, or environment.
* Provide a network that follows guests as they migrate.
* Separate traffic without configuring VLANs on every node.

A VNet is the unit of separation in SDN. Use additional VNets to isolate workloads, rather than additional [zones](Zones.md).

---

## Prerequisites

* A zone exists and has been applied successfully.
* The cluster has quorum.
* You know which zone the VNet belongs in.
* For a VLAN zone, you know the VLAN tag.
* If adding a subnet, you know the range and gateway.

---

# Procedure

## Step 1: Open the VNets Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter** in the resource tree.
3. Expand **SDN**.
4. Click **VNets**.

Existing VNets are listed with their zone and tag.

---

### Screenshot 1

**SDN VNets Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → SDN → VNets, showing the VNet list with zone and tag, the
> subnet panel beneath, and the **Create**, **Edit**, and **Remove** controls.

---

## Step 2: Create the VNet

1. Click **Create**.
2. Enter a **Name**. This becomes the bridge name guests attach to, so keep it short and meaningful.
3. Select the **Zone**.
4. For a VLAN or QinQ zone, enter the **Tag**.
5. Optionally set an **Alias** as a longer description.
6. Set **VLAN Aware** if guests will themselves use VLAN tags inside this VNet.
7. Confirm.

The name is what administrators will see in every guest's network configuration from now on. `tenant-a` reads better than `vnet1`.

---

### Screenshot 2

**Create VNet Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create VNet dialog, showing Name, Zone, Tag, Alias, and the VLAN
> Aware option.

---

## Step 3: Add a Subnet (Optional)

A VNet works without a subnet — guests can use it as a plain layer 2 network and manage addressing themselves. Add a subnet when you want SDN to know about the IP range, provide a gateway, or manage addresses.

1. Select the VNet.
2. In the subnet panel, click **Create**.
3. Enter the **Subnet** in CIDR form, for example `10.50.0.0/24`.
4. Enter the **Gateway**, for example `10.50.0.1`.
5. Set **SNAT** if guests should reach external networks through the gateway.
6. Optionally configure DHCP ranges, if address management is in use.
7. Confirm.

---

### Screenshot 3

**Create Subnet Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create Subnet dialog for a VNet, showing the subnet, gateway, and
> SNAT fields.

---

## Step 4: Apply

1. Return to the **SDN** panel.
2. Click **Apply**.
3. Wait for the task to complete on every node.
4. Confirm no pending changes remain.

Until this runs, the VNet does not exist on any node and will not appear when configuring a guest.

---

## Step 5: Attach a Guest

**Virtual machine:**

1. Select the machine, click **Hardware**.
2. Add or edit a network device.
3. Select the VNet as the **Bridge**.
4. Confirm.

**Container:**

1. Select the container, click **Network**.
2. Add or edit an interface.
3. Select the VNet as the **Bridge**.
4. Confirm.

See [Manage VM Hardware](../../04-Virtual-Machines/Manage-VM-Hardware.md) and [CT Network](../../05-Containers/CT-Network.md).

---

### Screenshot 4

**VNet Selected as a Guest Bridge**

```text
[ Place Screenshot Here ]
```

> **Capture:** A guest network device dialog with the Bridge dropdown open, showing the
> VNet listed alongside conventional bridges.

---

## Step 6: Verify Connectivity

1. Attach two guests on **different nodes** to the same VNet.
2. Confirm they reach each other.
3. Transfer something large between them, not just a ping.
4. Confirm a guest on a different VNet cannot reach them.
5. Migrate one guest and confirm it stays connected.

Step 3 is what catches MTU problems on VXLAN zones. A ping succeeding proves very little.

---

## Step 7: Edit or Remove

**To edit:** select the VNet, click **Edit**, change the fields, and Apply.

**To remove:**

1. Detach every guest using the VNet.
2. Remove its subnets.
3. Select the VNet and click **Remove**.
4. Apply.

> **Warning:** Removing a VNet disconnects every guest still attached to it, immediately on Apply. Move the guests first.

---

# Configuration / Options

### VNet

| Option | Description |
|---|---|
| **Name** | Identifier, and the bridge name guests attach to. Keep it short and meaningful. |
| **Zone** | The zone providing transport. Fixed at creation. |
| **Tag** | VLAN tag, for VLAN and QinQ zones. |
| **Alias** | Longer description. |
| **VLAN Aware** | Allows guests to use their own VLAN tags inside this VNet. |

### Subnet

| Option | Description |
|---|---|
| **Subnet** | IP range in CIDR form. |
| **Gateway** | Gateway address for the range. |
| **SNAT** | Whether guests reach external networks through the gateway. |
| **DHCP range** | Address range handed out, when address management is in use. |
| **DNS zone prefix** | Optional DNS integration. |

> **Verify:** Capture the Create VNet and Create Subnet dialogs and confirm the exact
> field labels and which options are present in this deployment.

---

# Verification

Verify the following:

* The VNet appears with the correct zone and tag.
* Apply completed on every node with no pending changes.
* The VNet is selectable as a bridge on guests.
* Two guests on the same VNet, on different nodes, reach each other.
* Large transfers succeed between them.
* Guests on different VNets are isolated.
* A subnet gateway is reachable, if configured.
* SNAT works, if enabled.
* A migrated guest keeps its connectivity.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| VNet not selectable on a guest | Apply has not run, or the zone does not cover that node. |
| Guests on the same VNet cannot reach each other | Same node or different nodes? If different, the problem is the zone transport, not the VNet. |
| Pings work, large transfers fail | MTU on a VXLAN or EVPN zone. See [Zones](Zones.md). |
| Guests reach each other but not external networks | No gateway, or SNAT is not enabled on the subnet. |
| Guest loses network after migrating | The zone does not include the target node. |
| Cannot remove the VNet | Guests are still attached, or subnets still exist. |
| VLAN tag has no effect | The zone type may not use tags. Simple and VXLAN zones do not. |
| Guest VLAN tagging not working | **VLAN Aware** is not enabled on the VNet. |
| Two VNets can reach each other unexpectedly | They may share a subnet range, or routing is configured at the zone level. |

---

# Best Practices

- Name VNets after what they carry — `tenant-a`, `dmz`, `storage-mgmt` — since the name appears in every guest's configuration.
- Use VNets, not zones, as the unit of isolation.
- Add subnets only when you want SDN managing addressing. A plain layer 2 VNet is often enough.
- Always test across **two nodes**, since single-node tests do not exercise the zone transport.
- Test with large transfers, not just pings.
- Verify migration keeps guests connected before moving production workloads.
- Detach guests before removing a VNet.
- Keep a record of which VNet belongs to which tenant or project.
- Remember to Apply. Nothing takes effect until you do.

---

# Related Documentation

- [SDN Overview](SDN-Overview.md)
- [Zones](Zones.md)
- [Manage VM Hardware](../../04-Virtual-Machines/Manage-VM-Hardware.md)
- [CT Network](../../05-Containers/CT-Network.md)
- [Network Overview](../../03-Nodes/System/Network/Network-Overview.md)
- [Manage Linux Bridge](../../03-Nodes/System/Network/Manage-Linux-Bridge.md)
- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)
- [Migrate Virtual Machine](../../04-Virtual-Machines/Migrate-Virtual-Machine.md)

---

# Summary

A VNet is the virtual network guests attach to, living inside a zone that carries its traffic between nodes. Once created and applied it appears as a bridge in every guest's network configuration, and it is the right unit of isolation in SDN — use more VNets rather than more zones to separate workloads.

Subnets are optional; a VNet works as a plain layer 2 network if guests manage their own addressing. Test connectivity between guests on **different nodes**, with large transfers rather than pings, since a single-node test never exercises the zone transport and a ping never reveals an MTU problem.
