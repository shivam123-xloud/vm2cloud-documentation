# Ceph Overview

---

## Overview

**Ceph** is a distributed storage system built into VM2Cloud VE. Instead of each node keeping guest disks on its own local storage, Ceph pools the disks across every node into one shared storage layer that all nodes can use.

That difference is what makes it worth the complexity. With Ceph, a guest's disk is not tied to the node running it, so live migration moves only memory, and a node failure does not take its guests' data down with it.

Ceph replaces the need for an external SAN or NAS. It also replaces the need for [replication](../Replication/Replication-Overview.md), which exists precisely because local storage is not shared.

> **Warning:** Ceph is the most demanding feature in VM2Cloud VE to run well. It needs a minimum of three nodes, benefits strongly from a dedicated high-speed network, and is unforgiving of undersized hardware. A poorly resourced Ceph cluster performs badly and can lose data. Do not deploy it on two nodes, on a shared 1 Gbps network, or without understanding the components below.

---

## When to Use

Use Ceph when:

* You have three or more nodes, ideally five or more.
* Guests need shared storage without an external SAN.
* Live migration should not require copying disks.
* Storage should survive the loss of a node.
* Capacity needs to grow by adding disks or nodes.
* You have, or can build, a dedicated storage network.

Do **not** use Ceph when:

* You have fewer than three nodes.
* Only a 1 Gbps network is available for storage traffic.
* The environment is small enough that [replication](../Replication/Replication-Overview.md) meets the requirement.
* Nobody will own it. Ceph needs monitoring and occasional intervention.

For a two- or three-node environment with modest needs, local storage plus replication is simpler, cheaper, and easier to reason about.

---

## Prerequisites

Before deploying Ceph, ensure that:

* The cluster has at least **three nodes** and quorum.
* Each node has **dedicated disks** for Ceph — whole disks with no existing data.
* A **separate network** exists for Ceph traffic, 10 Gbps or faster.
* Nodes have sufficient RAM. Each OSD consumes several gigabytes.
* Time is synchronized across all nodes. See [Time and NTP](../../03-Nodes/System/Time-and-NTP.md).
* You have capacity for **three copies** of your data — usable space is roughly one third of raw.
* You understand this is a long-lived commitment, not an experiment.

> **Warning:** Disks given to Ceph are wiped. Anything on them is destroyed. Confirm each disk is genuinely unused before assigning it.

---

# How Ceph Works

## The Components

| Component | Role | How many |
|---|---|---|
| **Monitor (MON)** | Maintains the cluster map and decides which nodes are in the cluster. | An odd number, normally 3 or 5. |
| **Manager (MGR)** | Provides metrics, status, and management interfaces. | At least 2, for redundancy. |
| **OSD** | One per disk. Stores the actual data and handles replication. | As many as you have disks. |
| **Metadata Server (MDS)** | Required only for CephFS file storage. | 2 or more if using CephFS. |

Monitors are the part people get wrong. They form their own quorum, separate from the VM2Cloud VE cluster quorum, and they need an **odd** number for the same reason. Two monitors are worse than one.

## How Data Is Stored

```text
            Guest disk
                |
                v
        Ceph pool (RBD)
                |
                v
      Split into objects
                |
                v
   CRUSH algorithm chooses placement
                |
                v
   Written to 3 OSDs on 3 different nodes
```

Ceph does not keep a primary copy with backups. Every copy is equal, and the CRUSH algorithm decides where they live, deliberately spreading them across different nodes so no single node failure takes out more than one copy.

## Replication Size

Two numbers control durability.

| Setting | Meaning | Standard value |
|---|---|---|
| **size** | Total copies of each object. | 3 |
| **min_size** | Copies required for the pool to accept writes. | 2 |

With size 3 / min_size 2, you can lose one node and keep writing. Lose two and the pool goes read-only to protect the remaining copy.

> **Warning:** Reducing **size** to 2 or **min_size** to 1 roughly doubles usable capacity and is a common temptation on small clusters. It also means a single disk failure during recovery can lose data permanently. Keep size 3 / min_size 2 for anything you care about.

## Usable Capacity

With size 3, usable capacity is about one third of raw capacity — and you should not run Ceph above roughly 80% full, because it needs free space to re-replicate after a failure.

```text
raw capacity  ÷  3  ×  0.8  ≈  usable capacity
```

Twelve 4 TB disks — 48 TB raw — gives roughly 12–13 TB usable. Plan for that before buying hardware.

## Health States

| State | Meaning |
|---|---|
| **HEALTH_OK** | Everything is as it should be. |
| **HEALTH_WARN** | Degraded but operating. Data is safe; something needs attention. |
| **HEALTH_ERR** | Data is at risk or unavailable. Act immediately. |

`HEALTH_WARN` after a disk failure is normal while Ceph re-replicates. `HEALTH_WARN` that persists for days is not.

---

# The Ceph Panels

Ceph appears at two levels:

| Level | Covers |
|---|---|
| **Datacenter → Ceph** | Cluster-wide health, pools, and configuration. |
| **Node → Ceph** | The monitors, managers, and OSDs on that node. See [Node Ceph](../../03-Nodes/Node-Ceph.md). |

Sub-pages here cover [Monitors and OSDs](Ceph-Monitors-and-OSDs.md) and [Pools](Ceph-Pools.md).

---

### Screenshot 1

**Datacenter Ceph Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Ceph on a healthy cluster, showing the health status, usage
> summary, and the monitor, manager, and OSD counts.

---

# Configuration / Options

Deployment is a multi-step process performed once per cluster:

1. Install the Ceph packages on every node.
2. Create the initial configuration, specifying the cluster network.
3. Create monitors — an odd number, on separate nodes.
4. Create managers.
5. Create OSDs from the dedicated disks. See [Monitors and OSDs](Ceph-Monitors-and-OSDs.md).
6. Create pools. See [Pools](Ceph-Pools.md).
7. Add the pool as VM2Cloud VE storage so guests can use it.

> **Verify:** Capture the Ceph installation wizard and the initial configuration dialog,
> and confirm the exact step sequence and field labels in this deployment. Also confirm
> whether installation is offered through the interface or requires `pveceph install`
> on each node.

The setting chosen at step 2 that matters most is the **cluster network** — the separate network Ceph uses for replication traffic. Getting this wrong means replication competes with guest traffic, and changing it later is disruptive.

---

# Verification

Verify the following:

* Datacenter → Ceph reports **HEALTH_OK**.
* The expected number of monitors is running, and it is odd.
* At least two managers exist, with one active.
* Every OSD is `up` and `in`.
* Pools show the intended size and min_size.
* Usable capacity matches expectations for size 3.
* Overall usage is below 80%.
* Ceph traffic is on the dedicated network, not the management network.
* A test guest on Ceph storage migrates between nodes without copying its disk.

That last check is the point of the whole exercise — if migration still copies disks, the guest is not actually on Ceph storage.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| HEALTH_WARN after a disk failure | Normal while re-replicating. It should clear on its own. Investigate if it persists. |
| HEALTH_WARN that will not clear | Often insufficient free space to re-replicate, or an OSD that is down. Check OSD status. |
| Pool is read-only | Available copies dropped below **min_size**. Restore the failed OSDs or nodes. |
| Performance is poor | Usually the network. Confirm Ceph traffic is on its dedicated network and not competing with guests. |
| Cluster fills faster than expected | Size 3 means every gigabyte written consumes three. |
| Monitors keep losing quorum | An even number of monitors, or network instability between nodes. |
| OSD will not start | Check the disk itself and the node's system log. See [Node Ceph](../../03-Nodes/Node-Ceph.md). |
| Cannot create OSDs | The disk may hold an existing partition table or filesystem. It must be unused. |
| Recovery is very slow | Ceph throttles recovery to protect client I/O. Expect hours on large clusters. |

---

# Best Practices

- **Three nodes is the minimum; five or more is where Ceph behaves well.** Recovery on a three-node cluster has nowhere to go when one node is down.
- Give Ceph a **dedicated network** of 10 Gbps or faster. This is the single largest factor in whether it performs acceptably.
- Keep **size 3 / min_size 2**. The capacity saving from reducing it is not worth the risk.
- Plan capacity as raw ÷ 3, then stay below 80%.
- Use an odd number of monitors — three for most clusters, five for large ones.
- Give Ceph whole disks, never partitions shared with anything else.
- Monitor health actively. `HEALTH_WARN` that nobody notices becomes `HEALTH_ERR`.
- Keep time synchronized. Ceph is sensitive to clock skew.
- **Back up guests on Ceph storage.** Ceph protects against hardware failure, not against deletion or corruption. See [Backup Jobs Overview](../Backup/Backup-Jobs-Overview.md).
- Do not deploy Ceph without someone owning it.

---

# Related Documentation

- [Ceph Monitors and OSDs](Ceph-Monitors-and-OSDs.md)
- [Ceph Pools](Ceph-Pools.md)
- [Node Ceph](../../03-Nodes/Node-Ceph.md)
- [Storage Overview](../Storage/Storage-Overview.md)
- [Storage Types](../Storage/Storage-Types.md)
- [Replication Overview](../Replication/Replication-Overview.md)
- [Backup Jobs Overview](../Backup/Backup-Jobs-Overview.md)
- [Cluster Overview](../Cluster/Cluster-Overview.md)
- [Network Overview](../../03-Nodes/System/Network/Network-Overview.md)

---

# Summary

Ceph pools the disks across every cluster node into one shared storage layer, so guest disks are no longer tied to the node running them. That gives fast live migration and survives node failure without an external SAN.

It is also the most demanding feature to run well. It needs at least three nodes, a dedicated 10 Gbps network, whole disks, and roughly three times the raw capacity of the data you intend to store. Keep size 3 / min_size 2, stay below 80% full, use an odd number of monitors, and remember that Ceph protects against hardware failure but not against deletion — guests on Ceph still need backups.
