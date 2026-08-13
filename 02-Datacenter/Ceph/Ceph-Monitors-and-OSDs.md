# Ceph Monitors and OSDs

---

## Overview

This page covers the two component types you create and manage most often in a Ceph cluster: **monitors**, which decide what the cluster looks like, and **OSDs**, which store the data.

Managers are covered here too, since they are created alongside monitors.

For what Ceph is, how data is placed, and whether you should be running it at all, see [Ceph Overview](Ceph-Overview.md).

| Component | Stores data | Quorum | Typical count |
|---|---|---|---|
| **Monitor (MON)** | No | Yes — needs an odd number | 3, or 5 on large clusters |
| **Manager (MGR)** | No | No — one active, others standby | 2 or 3 |
| **OSD** | Yes — one per disk | No | One per dedicated disk |

---

## When to Use

Work with monitors and OSDs when you need to:

* Deploy Ceph for the first time.
* Add disks to expand capacity.
* Replace a failed disk.
* Add or remove monitors, usually when adding or removing nodes.
* Investigate why the cluster is degraded.
* Remove a node from Ceph before decommissioning it.

---

## Prerequisites

* The cluster has quorum and Ceph is installed.
* For OSDs: dedicated, **completely unused** disks.
* For monitors: the node is on the Ceph network.
* You understand that adding or removing OSDs triggers data movement across the cluster.

---

# Monitors

## What They Do

Monitors hold the cluster map — which nodes exist, which OSDs are up, and where data should live. Every client consults a monitor before reading or writing.

They form **their own quorum**, separate from the VM2Cloud VE cluster quorum. If monitors lose quorum, Ceph stops serving I/O entirely, even though the VM2Cloud VE cluster may be perfectly healthy.

> **Warning:** Always run an **odd** number of monitors. Two monitors are strictly worse than one: either failing loses quorum, so you have doubled the failure probability while gaining nothing.

## Step 1: Create a Monitor

1. Select the node in the resource tree.
2. Expand **Ceph** and open the monitor panel.
3. Click **Create**.
4. Select the node.
5. Confirm.

Place monitors on **different nodes**. Three monitors on three nodes tolerates one node failure; three on one node tolerates none.

---

### Screenshot 1

**Ceph Monitor Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Ceph monitor panel showing three monitors on three different nodes,
> all in quorum, with the **Create**, **Start**, **Stop**, and **Destroy** controls.

---

## Step 2: Create Managers

Managers provide metrics and status. Ceph runs one active manager with the others on standby.

1. Open the manager panel.
2. Click **Create**.
3. Select a node.
4. Confirm.

Create at least two, on different nodes. With only one, losing that node loses cluster status reporting — Ceph keeps serving data, but you lose visibility exactly when you need it.

---

## Step 3: Remove a Monitor

1. Select the monitor.
2. Click **Destroy**.
3. Confirm.

> **Warning:** Removing a monitor changes the quorum requirement. Remove them one at a time, confirm quorum is healthy after each, and never drop to an even number and leave it there.

---

# OSDs

## What They Do

An OSD manages one disk. It stores objects, participates in replication, and reports its state to the monitors.

Each OSD has two independent states:

| State | Meaning |
|---|---|
| **up / down** | Whether the OSD process is running. |
| **in / out** | Whether Ceph is placing data on it. |

The combination matters. An OSD that is `down` but still `in` means Ceph expects data there and is waiting. Marking it `out` tells Ceph to re-replicate that data elsewhere — which starts recovery, and consumes free space.

## Step 4: Create an OSD

1. Select the node.
2. Expand **Ceph** and open the OSD panel.
3. Click **Create: OSD**.
4. Select the **disk**.
5. Optionally select separate devices for the database and write-ahead log — placing these on a faster device improves performance on spinning disks.
6. Confirm.

> **Warning:** Creating an OSD **wipes the selected disk**. Everything on it is destroyed. Confirm the disk is genuinely unused — check [View Disk Information](../../03-Nodes/Disks/View-Disk-Information.md) first.

---

### Screenshot 2

**Create OSD Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create OSD dialog, showing the disk selector and the optional DB and
> WAL device fields.

---

### Screenshot 3

**OSD List**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Ceph OSD panel showing OSDs across several nodes with their up/in
> state, usage, and the tree grouping them by host.

---

## Step 5: Expand Capacity

Add disks the same way. Ceph rebalances automatically, moving data onto the new OSDs.

Rebalancing generates substantial traffic and takes hours on a large cluster. Add capacity during a quiet period, and add disks in balanced groups across nodes rather than filling one node first — uneven capacity means CRUSH cannot place copies evenly.

---

## Step 6: Replace a Failed Disk

1. Identify the failed OSD — it shows `down`.
2. Mark it **out**, so Ceph re-replicates its data elsewhere.
3. Wait for the cluster to return to `HEALTH_OK`.
4. **Stop** the OSD.
5. **Destroy** it.
6. Physically replace the disk.
7. Create a new OSD on the replacement.
8. Wait for rebalancing to finish.

Do not rush steps 2 and 3. Destroying an OSD before its data has been re-replicated reduces your copy count, and doing that twice loses data.

> **Warning:** Never destroy more than one OSD at a time, and never destroy a second while the cluster is still recovering from the first. With size 3, two simultaneous losses leaves one copy; a third loses the data.

---

### Screenshot 4

**Failed OSD**

```text
[ Place Screenshot Here ]
```

> **Capture:** The OSD panel showing an OSD in a down state, with the **Out**, **Stop**,
> and **Destroy** actions available.

---

## Step 7: Remove a Node from Ceph

Before decommissioning a node:

1. Mark all its OSDs **out**, one at a time.
2. Wait for `HEALTH_OK` after each.
3. Stop and destroy each OSD.
4. Remove any monitor and manager on that node.
5. Confirm the cluster is healthy and quorum is odd.
6. Then proceed with [Remove Node from Cluster](../Cluster/Remove-Node-from-Cluster.md).

---

# Configuration / Options

### Monitor

| Option | Description |
|---|---|
| **Node** | Which node hosts the monitor. Spread them across nodes. |

### OSD

| Option | Description |
|---|---|
| **Disk** | The device to use. Wiped on creation. |
| **DB device** | Optional faster device for OSD metadata. Improves spinning-disk performance. |
| **WAL device** | Optional faster device for the write-ahead log. |
| **Encryption** | Whether to encrypt the OSD. Decide at creation; it cannot be changed later. |

> **Verify:** Capture the Create OSD dialog and confirm the exact field labels, and
> whether encryption is offered in this deployment.

---

# Verification

Verify the following:

* The monitor count is odd and all monitors are in quorum.
* Monitors are on different nodes.
* At least two managers exist, one active.
* Every OSD shows `up` and `in`.
* OSD count matches the disks you assigned.
* Capacity is distributed evenly across nodes.
* Cluster health is `HEALTH_OK`.
* After adding or removing an OSD, rebalancing completed and health returned to OK.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Monitors lose quorum | An even number, or network problems on the Ceph network. Return to an odd count. |
| OSD will not start | Check the physical disk and the node's system log. The disk may be failing. |
| Cannot create an OSD | The disk holds an existing partition table or filesystem. It must be entirely unused. |
| Cluster stuck in HEALTH_WARN | Often not enough free space to re-replicate. Add capacity or remove data. |
| Recovery extremely slow | Expected on large clusters. Ceph throttles recovery to protect client I/O. |
| Uneven usage across OSDs | Capacity is unbalanced across nodes. Add disks to the smaller nodes. |
| Data movement after adding a disk | Expected. Ceph rebalances to use the new capacity. |
| Pool went read-only during recovery | Copies dropped below min_size. Restore OSDs urgently. |
| Lost data after replacing disks | Almost always caused by destroying OSDs before recovery completed. |

---

# Best Practices

- **Odd number of monitors, on different nodes.** Three for most clusters.
- Run at least two managers.
- Give OSDs whole, unused disks.
- Add capacity in balanced groups across nodes, not one node at a time.
- Mark a failed OSD `out` and wait for `HEALTH_OK` **before** destroying it.
- Never work on two OSDs simultaneously.
- Add or remove capacity during quiet periods — rebalancing is I/O heavy.
- Put DB and WAL on faster devices when using spinning disks.
- Check [View Disk Information](../../03-Nodes/Disks/View-Disk-Information.md) before assigning any disk.
- Remove a node from Ceph fully before removing it from the cluster.

---

# Related Documentation

- [Ceph Overview](Ceph-Overview.md)
- [Ceph Pools](Ceph-Pools.md)
- [Node Ceph](../../03-Nodes/Node-Ceph.md)
- [View Disk Information](../../03-Nodes/Disks/View-Disk-Information.md)
- [Disks Overview](../../03-Nodes/Disks/Disks-Overview.md)
- [Remove Node from Cluster](../Cluster/Remove-Node-from-Cluster.md)
- [Storage Overview](../Storage/Storage-Overview.md)

---

# Summary

Monitors hold the cluster map and form their own quorum, so they must be an odd number spread across different nodes — two monitors are worse than one. Managers provide status and should number at least two. OSDs each manage one disk and carry two independent states, `up`/`down` and `in`/`out`.

The operation that most often goes wrong is replacing a failed disk. Mark the OSD `out`, wait for the cluster to return to `HEALTH_OK`, and only then stop and destroy it. Destroying an OSD before its data has been re-replicated reduces your copy count, and doing that to a second disk during recovery is how Ceph clusters lose data.
