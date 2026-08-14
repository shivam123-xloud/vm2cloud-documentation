# Fabrics

---

## Overview

A **fabric** automatically configures a routing protocol across your nodes' physical interfaces, so the nodes can route to each other without anyone hand-configuring the underlay.

This solves a specific problem. VXLAN and EVPN zones need every node to reach every other node by IP. On a flat layer 2 network that is free. On a routed network — a spine-leaf data centre design, or nodes in different racks or sites — someone has to make that routing work. A fabric does it for you.

If your nodes already reach each other on a single subnet, you do not need a fabric.

For the SDN model as a whole, see [SDN Overview](SDN-Overview.md).

---

## When to Use

Create a fabric when:

* Nodes sit in different layer 3 segments and must route to each other.
* You are building a spine-leaf topology.
* VXLAN or EVPN needs a routed underlay.
* Nodes span sites and need encrypted connectivity between them.

Do **not** create one when all nodes share a subnet and can already reach each other. The underlay is already working; a fabric adds complexity for nothing.

---

## Prerequisites

* The cluster has quorum.
* You know the physical interfaces that will carry fabric traffic.
* You have a unique router ID for each node.
* You know the IP prefix range the router IDs fall within.
* The physical network passes the routing protocol between nodes.
* For a site-to-site fabric, you have the keys or credentials it requires.

> **Warning:** A fabric reconfigures routing on the interfaces you assign to it. Do not assign the interface carrying management traffic unless you intend the fabric to route it — an error there makes the node unreachable and needs console access to recover.

---

# Protocols

| Protocol | Basis | Best for |
|---|---|---|
| **OpenFabric** | IS-IS | Spine-leaf data centre topologies. Handles IPv4 and IPv6. |
| **OSPF** | Link-state | Hierarchical networks, and environments already running OSPF. |
| **WireGuard** | Encrypted tunnel | Connecting nodes across sites over untrusted networks. |

**OpenFabric** is the usual choice for a data centre fabric — it is designed for the many-equal-paths shape a spine-leaf network has.

**OSPF** fits where the wider network already speaks OSPF and the fabric should participate in it.

**WireGuard** is for reaching nodes over the internet or another network you do not control, since it encrypts the traffic between them.

> **Verify:** Confirm which protocols the **Add Fabric** dropdown offers in this
> deployment.

---

# Procedure

## Step 1: Open the Fabrics Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Expand **SDN**.
4. Click **Fabrics**.

---

### Screenshot 1

**Fabrics Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → SDN → Fabrics, showing the **Add Fabric** dropdown open with
> the available protocols, alongside the **Add Node** and **Reload** buttons and the
> Name, Protocol, IPv4, IPv6, Interfaces, Action, and State columns.

---

## Step 2: Create the Fabric

1. Click **Add Fabric** and select the protocol.
2. Enter a **Name**.
3. Enter the **IPv4** and, if used, **IPv6** prefix that router IDs must fall within.
4. Set the protocol's timing values, or leave the defaults.
5. Confirm.

> **Note:** The fabric name is short — around eight characters. Choose something that will still make sense as an identifier, such as `dc1` or `site-a`.

---

### Screenshot 2

**Create Fabric Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Fabric dialog for OpenFabric, showing the Name, IPv4 and IPv6
> prefix fields, and any timing options.

---

## Step 3: Add Each Node

A fabric with no nodes does nothing. Add every node that should participate.

1. Click **Add Node**.
2. Select the node.
3. Enter its **Router ID** — a unique address inside the prefix set on the fabric.
4. Select the **interfaces** carrying fabric traffic.
5. Optionally set per-interface addressing.
6. Confirm.
7. Repeat for every node.

Two things matter here:

**Router IDs must be unique.** Two nodes sharing one produces a routing adjacency that forms and then breaks, intermittently.

**Interfaces must be the ones physically cabled for it.** Assigning an interface that is not connected to the fabric network gives you a node that appears configured and never forms an adjacency.

---

### Screenshot 3

**Add Node to Fabric**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Node dialog, showing the node selector, Router ID field, and
> interface selection.

---

## Step 4: Apply

Return to the **SDN** panel and click **Apply**.

Fabric configuration is staged like the rest of SDN. The **State** column shows entries as pending until this runs.

---

### Screenshot 4

**Fabric Configured**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Fabrics panel after Apply, showing a configured fabric with its nodes,
> protocol, and applied state.

---

## Step 5: Verify Routing

1. Confirm each node appears in the fabric with its router ID.
2. From one node, reach another node's router ID.
3. Confirm the cluster stays quorate.
4. If the fabric is the underlay for VXLAN or EVPN, confirm guests on those networks reach each other across nodes.

Test node-to-node reachability directly before relying on it for an overlay. A fabric problem looks identical to an SDN zone problem from the guest's point of view.

---

# Configuration / Options

### Fabric

| Field | Description |
|---|---|
| **Name** | Short identifier, around eight characters. |
| **Protocol** | OpenFabric, OSPF, or an encrypted tunnel. Fixed at creation. |
| **IPv4 prefix** | Range that router IDs must fall within. |
| **IPv6 prefix** | Same for IPv6, where used. |
| **Hello interval** | How often adjacency messages are sent. |
| **CSNP interval** | Database synchronisation interval, for OpenFabric. |

### Node

| Field | Description |
|---|---|
| **Node** | The cluster node joining the fabric. |
| **Router ID** | Unique address identifying this node. Must be inside the fabric prefix. |
| **Interfaces** | Physical interfaces carrying fabric traffic. |

> **Verify:** Capture both dialogs and confirm the exact field labels, the name length
> limit, and the default timing values.

---

# Verification

Verify the following:

* The fabric appears with the intended protocol.
* Every participating node is listed.
* Router IDs are unique and inside the fabric prefix.
* Interfaces match what is physically cabled.
* **Apply** completed with no pending state.
* Nodes reach each other's router IDs.
* The cluster remains quorate.
* Overlay networks built on the fabric work across nodes.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Nodes do not form an adjacency | Check the assigned interfaces are the ones actually cabled, and that the physical network passes the protocol. |
| Intermittent routing | Duplicate router IDs. Each node needs its own. |
| Router ID rejected | It falls outside the fabric's configured prefix. |
| Node became unreachable after Apply | The management interface was assigned to the fabric. Recover from the console. |
| Fabric shows pending | **Apply** has not run. |
| VXLAN works on some nodes only | The fabric is incomplete — a node was never added, or its adjacency is down. |
| Cluster lost quorum | Cluster traffic is affected by the routing change. Confirm the cluster network path. |
| Name rejected | The name exceeds the length limit. |

---

# Best Practices

- **Only build a fabric if the underlay genuinely needs routing.** Nodes on one subnet do not.
- Never assign the management interface to a fabric unless that is deliberate.
- Plan router IDs before configuring — a simple scheme such as one per node inside a dedicated prefix avoids duplicates.
- Use dedicated interfaces for fabric traffic where possible.
- Add every node before relying on the fabric; a partial fabric fails only for the guests that land on the missing node.
- Verify node-to-node reachability directly before building an overlay on top.
- Keep console access available while configuring.
- Choose OpenFabric for spine-leaf, OSPF where the network already runs it.

---

# Related Documentation

- [SDN Overview](SDN-Overview.md)
- [Zones](Zones.md)
- [VNets](VNets.md)
- [SDN Options](SDN-Options.md)
- [Route Maps](Route-Maps.md)
- [Network Overview](../../03-Nodes/System/Network/Network-Overview.md)
- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)
- [Quorum](../Cluster/Quorum.md)

---

# Summary

A fabric configures a routing protocol across your nodes automatically, so they can reach each other on a routed network without hand-built underlay configuration. It exists to support VXLAN and EVPN overlays where nodes are not on a single flat network — and if your nodes already share a subnet, you do not need one.

Choose OpenFabric for spine-leaf topologies, OSPF where the surrounding network already speaks it, and an encrypted tunnel for nodes across sites. The two things that break fabrics are duplicate router IDs, which produce intermittent adjacencies, and interfaces assigned that are not physically cabled for the fabric — including, expensively, the management interface.
