# Node Replication

---

## Overview

The **Replication** tab on a node lists every replication job involving that node — both jobs replicating *from* it and, depending on the view, jobs replicating *to* it.

Where the guest tabs show one machine's jobs and the datacenter panel shows the whole cluster, this is the per-node view. It answers a question the other two do not: **what is this node responsible for replicating, and is it keeping up?**

That matters most when planning maintenance. Before taking a node down, you want to know which guests replicate from it and how current those copies are.

For what replication is and how jobs are configured, see [Replication Overview](../02-Datacenter/Replication/Replication-Overview.md).

---

## When to Use

Open the node Replication tab when you need to:

* See every replication job involving this node.
* Check replication health before taking the node down for maintenance.
* Confirm how current the replicas are before a planned failover.
* Assess replication load on a node.
* Investigate replication failures concentrated on one node.
* Confirm replication resumed after the node returned from an outage.

---

## Prerequisites

* You have permission to view the node.
* The node is part of a cluster.
* The node is online. An offline node cannot report job status.

---

# Procedure

## Step 1: Open the Node Replication Tab

1. Log in to the VM2Cloud VE web interface.
2. Expand **Datacenter** in the resource tree.
3. Select the node.
4. Click **Replication**.

Jobs involving this node are listed.

---

### Screenshot 1

**Node Replication Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A node → Replication, showing several jobs for different guests with their
> targets, schedules, last sync times, and status.

---

## Step 2: Review the Jobs

| Column | Meaning |
|---|---|
| **Guest** | The VM or container being replicated. |
| **Target** | The destination node. |
| **Schedule** | How often the job runs. |
| **Last Sync** | When it last completed successfully. |
| **Duration** | How long the last run took. |
| **Status** | `OK`, running, or an error. |

Scan the **Last Sync** column across all jobs. A single stale entry is a job problem; several stale entries on one node point at the node — its network path, its storage, or its load.

---

## Step 3: Check Before Maintenance

Before taking the node offline:

1. Confirm every guest that should be replicated has a job.
2. Confirm **Last Sync** is recent for each.
3. Trigger a manual run for anything stale, and let it complete.
4. Note which target node holds each replica.

Guests replicating *from* this node stop being protected while it is down. Guests replicating *to* it lose their replication target — their jobs will fail until it returns, and those failures are expected rather than a fault.

---

### Screenshot 2

**Replication Status Before Maintenance**

```text
[ Place Screenshot Here ]
```

> **Capture:** The node Replication tab with several jobs all showing recent successful
> syncs, as it should look before planned maintenance.

---

## Step 4: Assess Replication Load

Replication competes with production traffic and with storage I/O.

Look for:

* Many jobs scheduled at the same time.
* Durations approaching the interval between runs.
* Jobs whose duration is growing over time.

A job taking eight minutes on a ten-minute schedule has almost no headroom. Stagger schedules, lengthen intervals, or apply rate limits. See [Replication Scheduling](../02-Datacenter/Replication/Replication-Scheduling.md).

---

## Step 5: Investigate Failures

For a failing job:

1. Select it and review the error.
2. Open the node's [Task History](Task-History.md) for the full output.
3. Check whether the target node is online and has capacity.
4. Check connectivity between the two nodes.
5. Check the cluster has quorum.

Failures affecting every job on the node point at the node or the cluster, not at individual jobs. See [Replication Troubleshooting](../02-Datacenter/Replication/Replication-Troubleshooting.md).

---

# Configuration / Options

Jobs are configured the same way from any level. See [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md) for the field reference.

| Control | Purpose |
|---|---|
| **Add** | Create a job for a guest on this node. |
| **Edit** | Change an existing job. |
| **Remove** | Delete a job. |
| **Schedule now** | Trigger an immediate synchronization. |

> **Verify:** Confirm whether this tab shows only jobs replicating *from* this node, or
> also jobs replicating *to* it, in this deployment.

---

# Verification

Verify the following:

* Every guest on the node that should be replicated has a job.
* **Last Sync** is recent across all jobs.
* No job duration approaches its interval.
* No errors in the status column.
* After maintenance, jobs resumed and caught up.
* Target nodes hold current replicas.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Every job on the node is failing | The problem is the node or the cluster, not the jobs. Check connectivity, quorum, and storage. |
| One job failing, others fine | Job-specific. Check the target node's capacity for that guest. |
| Jobs stale after an outage | They should resume automatically. Trigger a manual run to catch up, then confirm the next scheduled run works. |
| Durations growing across all jobs | The node is under load, or the storage is slower than it was. Stagger schedules or apply rate limits. |
| Jobs disappeared after guests migrated away | Jobs follow the guest. They now appear on the guest's new node. |
| A guest on this node has no job | It was never configured, or the job was removed. Check against your protection requirements. |
| Jobs to this node fail while it is down | Expected. They resume when the node returns. |
| Tab is empty | No replication jobs involve this node, or the node is not clustered. |

---

# Best Practices

- Check this tab before every planned maintenance window, not just when something looks wrong.
- Trigger manual runs for stale jobs before taking the node down.
- Watch for durations approaching the schedule interval — that is the warning before jobs start overlapping.
- Stagger replication schedules across guests rather than running them together.
- Treat several stale jobs on one node as a node problem, not several job problems.
- Confirm replication caught up after any node outage.
- Review this tab after migrating guests on or off the node.
- Keep replication and backup separate in your planning; this tab shows only replication.

---

# Related Documentation

- [Replication Overview](../02-Datacenter/Replication/Replication-Overview.md)
- [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md)
- [Replication Scheduling](../02-Datacenter/Replication/Replication-Scheduling.md)
- [Replication Status](../02-Datacenter/Replication/Replication-Status.md)
- [Replication Troubleshooting](../02-Datacenter/Replication/Replication-Troubleshooting.md)
- [Task History](Task-History.md)
- [Reboot Node](Reboot-Node.md)
- [Shutdown Node](Shutdown-Node.md)
- [VM Replication](../04-Virtual-Machines/VM-Replication.md)
- [CT Replication](../05-Containers/CT-Replication.md)

---

# Summary

The node Replication tab lists every replication job involving one node, which is the view you want when planning maintenance or assessing load. Before taking a node down, confirm each job's **Last Sync** is recent and trigger a manual run for anything stale.

Read the column as a whole rather than job by job. One stale entry is a job problem; several on the same node point at the node itself — its network path, its storage, or simply too many jobs running at once.
