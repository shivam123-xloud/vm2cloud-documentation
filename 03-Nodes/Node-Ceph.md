# Node Ceph

---

## Overview

The **Ceph** section on a node shows the Ceph components running on that specific server — its monitors, managers, OSDs, and their state.

Where [Datacenter → Ceph](../02-Datacenter/Ceph/Ceph-Overview.md) shows cluster-wide health, this is the per-node view. It answers a different question: **what is this node contributing to Ceph, and is any of it broken?**

That is the view you want when a disk fails, when preparing a node for maintenance, or when cluster health is degraded and you need to find which node is responsible.

For Ceph concepts, deployment, and the component reference, see [Ceph Overview](../02-Datacenter/Ceph/Ceph-Overview.md) and [Monitors and OSDs](../02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md). This page covers the node-level panel.

---

## When to Use

Open the node Ceph section when you need to:

* See which OSDs live on this node and whether they are healthy.
* Identify a failed disk on a specific node.
* Check whether this node hosts a monitor or manager.
* Prepare the node for maintenance.
* Investigate degraded cluster health traced to one node.
* Remove Ceph components before decommissioning the node.

---

## Prerequisites

* Ceph is deployed on the cluster.
* The node is online and part of the cluster.
* You have permission to view or manage Ceph on the node.

---

# Procedure

## Step 1: Open the Node Ceph Section

1. Log in to the VM2Cloud VE web interface.
2. Expand **Datacenter** in the resource tree.
3. Select the node.
4. Expand **Ceph**.

Sub-panels for monitors, OSDs, and configuration appear.

---

### Screenshot 1

**Node Ceph Section**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node → Ceph expanded in the resource tree, showing the available
> sub-panels.

---

## Step 2: Review the OSDs on This Node

Open the OSD panel. Each OSD on this node is listed with its state and usage.

| Column | Meaning |
|---|---|
| **ID** | The OSD number. |
| **Status** | `up` or `down` — whether the process is running. |
| **In / Out** | Whether Ceph is placing data on it. |
| **Used** | How full it is. |
| **Latency** | Response times. Rising latency often precedes a disk failure. |

Look for anything `down`, and for usage that differs sharply from the other OSDs on the node.

---

### Screenshot 2

**Node OSD List**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node's Ceph OSD panel showing several OSDs with their up/in state, usage,
> and latency.

---

## Step 3: Check Monitors and Managers on This Node

Confirm whether this node hosts a monitor or manager, and whether they are running.

This matters before maintenance. Taking down a node that hosts one of three monitors leaves two — an even number, and one failure away from losing Ceph entirely.

> **Warning:** Before rebooting a node that hosts a monitor, confirm the remaining monitors still form an odd quorum. Three monitors minus one leaves two, which tolerates no further failure. Plan maintenance so only one monitor is down at a time.

---

## Step 4: Identify a Failed Disk

When cluster health reports a problem:

1. Open this node's OSD panel.
2. Find the OSD showing `down`.
3. Note its ID and which physical disk it uses.
4. Cross-check against [View Disk Information](Disks/View-Disk-Information.md) to identify the physical device.
5. Check the node's system log for disk errors. See [Syslog](System/Syslog.md).

Identifying the correct physical disk is the step to be careful with — pulling the wrong disk from a running Ceph node turns one failure into two.

Replacement is covered in [Monitors and OSDs](../02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md).

---

### Screenshot 3

**Failed OSD on a Node**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node's OSD panel showing an OSD in a down state.

---

## Step 5: Prepare the Node for Maintenance

Before rebooting or shutting down a node running Ceph:

1. Confirm cluster health is `HEALTH_OK`. Do not take a node down while the cluster is already degraded.
2. Check whether this node hosts a monitor, and confirm the remaining monitors stay odd.
3. Set the cluster to avoid rebalancing during the outage, so it does not start moving data for a node that will return in ten minutes.
4. Perform the maintenance.
5. Confirm all OSDs return `up` and `in`.
6. Re-enable rebalancing.
7. Wait for `HEALTH_OK`.

> **Verify:** Confirm how rebalancing is paused during maintenance in this deployment —
> whether the interface exposes maintenance flags, or whether `ceph osd set noout` is
> required from the shell.

Skipping step 3 means Ceph begins re-replicating everything on the node as soon as it goes down, generating hours of unnecessary traffic for a short reboot.

---

## Step 6: Remove Ceph From a Node

Before decommissioning:

1. Mark each OSD **out**, one at a time.
2. Wait for `HEALTH_OK` after each.
3. Stop and destroy each OSD.
4. Remove any monitor and manager on the node.
5. Confirm the remaining monitor count is odd.
6. Confirm cluster health is `HEALTH_OK`.
7. Proceed with [Remove Node from Cluster](../02-Datacenter/Cluster/Remove-Node-from-Cluster.md).

---

# Configuration / Options

The node panels expose the same actions as the cluster-level ones, scoped to this node.

| Panel | Actions |
|---|---|
| **OSD** | Create, Start, Stop, Out, In, Destroy |
| **Monitor** | Create, Start, Stop, Destroy |
| **Manager** | Create, Start, Stop, Destroy |
| **Configuration** | View the Ceph configuration as it applies to this node |

Field reference is in [Monitors and OSDs](../02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md).

> **Verify:** Capture each node-level Ceph sub-panel and confirm which are present in
> this deployment.

---

# Verification

Verify the following:

* All OSDs on the node are `up` and `in`.
* Usage is even across the node's OSDs.
* Latency is comparable to other nodes.
* Any monitor or manager here is running.
* Cluster health is `HEALTH_OK`.
* After maintenance, all OSDs returned and rebalancing was re-enabled.
* After removing components, the monitor count is still odd.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| One OSD is down | Check the physical disk and the node's system log. Replace following [Monitors and OSDs](../02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md). |
| All OSDs on the node are down | A node-level problem, not a disk problem. Check the Ceph network and the node's services. |
| Latency high on one OSD | The disk may be failing. Rising latency often precedes failure. |
| Cluster degraded after a reboot | Rebalancing was not paused, or an OSD did not return. Check all are `up` and `in`. |
| Ceph lost quorum during maintenance | The node hosted a monitor and the remainder was even. Plan monitor placement before maintenance. |
| Usage uneven across this node's OSDs | Disks of differing sizes, or too few placement groups. |
| Cannot create an OSD | The disk is not empty. See [View Disk Information](Disks/View-Disk-Information.md). |
| Node shows no Ceph panels | Ceph is not installed on this node. |

---

# Best Practices

- Check this panel before any maintenance on a Ceph node, not only when something is wrong.
- Never take a node down while the cluster is already degraded.
- Pause rebalancing for short maintenance, and remember to re-enable it afterwards.
- Confirm monitor quorum stays odd before removing a node from service.
- Watch OSD latency as an early failure signal.
- Identify the physical disk carefully before pulling one from a running node.
- Remove Ceph components fully before removing a node from the cluster.
- Keep the Ceph network healthy — most node-wide OSD problems are network problems.

---

# Related Documentation

- [Ceph Overview](../02-Datacenter/Ceph/Ceph-Overview.md)
- [Ceph Monitors and OSDs](../02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md)
- [Ceph Pools](../02-Datacenter/Ceph/Ceph-Pools.md)
- [View Disk Information](Disks/View-Disk-Information.md)
- [Disks Overview](Disks/Disks-Overview.md)
- [Syslog](System/Syslog.md)
- [Reboot Node](Reboot-Node.md)
- [Node Summary](Node-Summary.md)
- [Remove Node from Cluster](../02-Datacenter/Cluster/Remove-Node-from-Cluster.md)

---

# Summary

The node Ceph section shows what one server contributes to the Ceph cluster — its OSDs, and whether it hosts a monitor or manager. It is the view for identifying a failed disk and for preparing a node for maintenance.

Two things matter before taking a Ceph node down. Confirm the cluster is already healthy, since removing a node from a degraded cluster compounds the problem. And check whether the node hosts a monitor — three monitors minus one leaves two, which tolerates no further failure. Pause rebalancing for short outages so Ceph does not spend hours re-replicating data for a node that returns in minutes.
