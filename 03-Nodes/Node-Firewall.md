# Node Firewall

---

## Overview

The **Firewall** section on a node filters traffic to and from the node itself — the host, not the guests running on it.

This is the level that protects the management interface, SSH access, and cluster communication. It is also the level most likely to lock you out if configured carelessly, because the VM2Cloud VE web interface runs on the node.

Node firewall rules apply **in addition to** the datacenter rules. Traffic must be permitted at both levels to reach the node.

For the rule model, direction, actions, and evaluation order, see [Firewall Overview](../02-Datacenter/Firewall/Firewall-Overview.md). This page covers the node-level panel only.

> **Warning:** The VM2Cloud VE web interface listens on TCP port **8006** on the node, and SSH on port **22**. Blocking either at node level removes your remote access to that node. You will need console or physical access to recover. Never enable the node firewall without a verified management-access rule.

---

## When to Use

Configure the node firewall when:

* Management access to a node must be restricted to an administrative network.
* SSH access to the host should be limited.
* A node has a different exposure profile from the rest of the cluster.
* Host-level services need protecting independently of guest traffic.
* A compliance requirement calls for host-level filtering.

Use the [datacenter firewall](../02-Datacenter/Firewall/Firewall-Rules.md) instead when a rule should apply to every node.

---

## Prerequisites

Before configuring the node firewall, ensure that:

* You have administrator privileges.
* The datacenter-level firewall is enabled, or you understand that node rules do nothing until it is.
* You know which addresses must retain management access.
* **You have working console or physical access to the node.**
* You know the addresses of the other cluster nodes, so cluster traffic is not blocked.
* Any alias, IPSet, or security group the rules reference already exists.

---

# Procedure

## Step 1: Open the Node Firewall Panel

1. Log in to the VM2Cloud VE web interface.
2. Expand **Datacenter** in the resource tree.
3. Select the node.
4. Expand **Firewall**.
5. Click **Rules**.

The node's rule list is shown.

---

### Screenshot 1

**Node Firewall Rules Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node → Firewall → Rules, showing the rule list and the **Add**,
> **Edit**, **Remove**, and **Insert: Security Group** controls.

---

## Step 2: Add a Management-Access Rule First

Before enabling anything, permit your own access.

1. Click **Add**.
2. Set **Direction** to **in**.
3. Set **Action** to **ACCEPT**.
4. Set **Protocol** to `tcp`.
5. Set **Dest. port** to `8006`.
6. Set **Source** to your administrative network or an alias representing it.
7. Add a **Comment** such as `Management access to web interface`.
8. Confirm **Enable** is set.
9. Click **Add**.

Add a second rule the same way for SSH on port `22` if you need shell access.

---

### Screenshot 2

**Management Access Rule**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Rule dialog on a node, filled in for inbound TCP 8006 from a
> specific source network.

---

## Step 3: Permit Cluster Communication

If the node is part of a cluster, cluster traffic must not be blocked.

1. Add rules permitting traffic between this node and the other cluster node addresses on the cluster network.
2. Use an IPSet containing the cluster node addresses so one rule covers them all.

> **Warning:** Blocking cluster communication causes the node to lose quorum. The cluster file system becomes read-only and HA behaviour is affected.

> **Verify:** Confirm which ports must be permitted for cluster communication in this
> deployment, and whether a predefined macro exists for them.

---

### Screenshot 3

**Cluster Communication Rule**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node firewall rule permitting traffic from an IPSet containing the
> other cluster node addresses.

---

## Step 4: Add Service Rules

Add rules for any other service the node itself must expose. Follow the same pattern: direction, action, protocol, destination port, source.

Remember these rules protect the **host**. Rules for services running inside a guest belong on that guest.

---

## Step 5: Review the Rule Order

1. Confirm the management-access rule sits above any broader deny rule.
2. Confirm cluster rules are present and enabled.
3. Use the move controls to reorder if needed.

The first matching rule wins, so a broad deny above a specific accept will block the accept.

---

### Screenshot 4

**Node Rule List in Order**

```text
[ Place Screenshot Here ]
```

> **Capture:** The node Rules panel with several rules present, showing management,
> cluster, and service rules in evaluation order.

---

## Step 6: Enable the Node Firewall

1. Open a **second browser session** and leave it logged in.
2. Click **Options** under the node's **Firewall**.
3. Select the firewall enable setting.
4. Click **Edit**.
5. Enable it.
6. Click **OK**.

---

### Screenshot 5

**Node Firewall Options**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node → Firewall → Options, showing the full settings list including the
> enable setting and default policies.

---

## Step 7: Verify Access Immediately

1. From a **different machine**, load the web interface for this node.
2. Confirm it responds.
3. Confirm the node still shows as online in the cluster.
4. Confirm guests on the node are still reachable.

If access is lost, follow [Firewall Lockout Recovery](../02-Datacenter/Firewall/Firewall-Lockout-Recovery.md).

---

# Configuration / Options

The node **Options** panel controls firewall behaviour for this node only.

| Option | Description |
|---|---|
| **Firewall** | Enables filtering for this node. Node rules do nothing until this and the datacenter firewall are both enabled. |
| **Input Policy** | Action for inbound traffic no rule matches. Normally **DROP**. |
| **Output Policy** | Action for outbound traffic no rule matches. Normally **ACCEPT**. |
| **Log level (in)** | Logging detail for inbound traffic. |
| **Log level (out)** | Logging detail for outbound traffic. |

Rule fields are identical to the datacenter level — see [Firewall Rules](../02-Datacenter/Firewall/Firewall-Rules.md).

> **Verify:** Capture the complete node Firewall → Options list and confirm the exact
> setting names and defaults. Node-level options may differ from datacenter options.

---

# Verification

Verify the following:

* The web interface for this node is reachable from another machine.
* The node shows as **Online** in the cluster.
* The cluster reports quorum.
* SSH access works if a rule permits it.
* Guests on the node are still reachable.
* Blocked services fail as intended.
* Replication and backup jobs involving this node still complete.
* The firewall log shows traffic as expected.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Locked out of the node web interface | Use console or physical access, disable the node firewall, then add a management rule before retrying. |
| Node shows offline after enabling | Cluster communication is being blocked. Permit traffic between node addresses on the cluster network. |
| Cluster loses quorum | Same cause. Restore cluster traffic immediately; the cluster file system is read-only without quorum. |
| Node rules have no effect | The datacenter firewall is disabled. Node rules require it to be enabled. |
| SSH stops working | No rule permits inbound TCP 22, or a broader deny sits above the SSH rule. |
| Guests lose network access | A node rule is blocking traffic passing to the guests. Node rules protect the host; check the rule is not too broad. |
| Backups or replication fail | Traffic to the storage or target node is blocked. Permit it explicitly. |
| Cannot edit firewall settings | Confirm administrator privileges and cluster quorum. |

---

# Best Practices

- **Never enable the node firewall without console access available.**
- Add and verify the management-access rule before enabling anything.
- Keep a second browser session open during changes.
- Use an IPSet for cluster node addresses so cluster rules stay correct as nodes are added.
- Apply cluster-wide policy at the datacenter level; keep node rules for what is genuinely node-specific.
- Configure one node, verify it fully, then repeat on the others.
- Make changes during a maintenance window.
- Comment every rule with why it exists.
- Restrict management access by source rather than exposing port 8006 broadly.

---

# Related Documentation

- [Firewall Overview](../02-Datacenter/Firewall/Firewall-Overview.md)
- [Firewall Rules](../02-Datacenter/Firewall/Firewall-Rules.md)
- [Firewall Options](../02-Datacenter/Firewall/Firewall-Options.md)
- [Security Groups](../02-Datacenter/Firewall/Security-Groups.md)
- [Aliases](../02-Datacenter/Firewall/Aliases.md)
- [IPSets](../02-Datacenter/Firewall/IPSets.md)
- [Node Troubleshooting](Node-Troubleshooting.md)
- [Network Troubleshooting](System/Network/Network-Troubleshooting.md)
- [Quorum](../02-Datacenter/Cluster/Quorum.md)

---

# Summary

The node firewall protects the host itself — its management interface, SSH access, and cluster communication — and applies in addition to the datacenter rules. It is the level with the highest lockout risk, because the web interface runs on the node and the default input policy is **DROP**. Add and verify a management-access rule and cluster-communication rules before enabling it, keep console access available, and configure one node at a time.
