# VM Replication

---

## Overview

The **Replication** tab on a virtual machine shows the replication jobs configured for that machine, and lets you create, edit, and run them from the guest itself.

This is the same replication feature documented at datacenter level — the tab is a filtered view of it, scoped to one machine. Anything you create here appears in the datacenter list, and anything created there appears here if it concerns this machine.

For what replication is, how it differs from backup, and the full job configuration, see [Replication Overview](../02-Datacenter/Replication/Replication-Overview.md). This page covers the guest-level panel.

> **Note:** Replication requires a cluster and supported local storage. On a standalone node the tab is present but nothing can be configured.

---

## When to Use

Use the guest Replication tab when you need to:

* Check whether this machine is replicated, and to where.
* See when it last synchronized successfully.
* Add replication for this machine specifically.
* Trigger a synchronization immediately, before a planned migration.
* Investigate a replication failure affecting this machine.

Use the [datacenter Replication panel](../02-Datacenter/Replication/Replication-Overview.md) instead when reviewing replication across the whole cluster.

---

## Prerequisites

* You have permission to modify the machine.
* The node is part of a cluster with quorum.
* The machine's disks are on storage that supports replication.
* The target node has compatible storage with sufficient capacity.

---

# Procedure

## Step 1: Open the Replication Tab

1. Log in to the VM2Cloud web interface.
2. Expand the node in the resource tree.
3. Select the virtual machine.
4. Click **Replication**.

Jobs for this machine are listed.

---

### Screenshot 1

**VM Replication Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine → Replication, showing a configured job with its target,
> schedule, last sync, duration, and status, plus the **Add**, **Edit**, **Remove**, and
> **Schedule now** controls.

---

## Step 2: Read the Job List

| Column | Meaning |
|---|---|
| **Target** | The node the machine replicates to. |
| **Schedule** | How often the job runs. |
| **Last Sync** | When it last completed successfully. |
| **Next Sync** | When it will run again. |
| **Duration** | How long the last run took. |
| **Status** | `OK`, running, or an error. |

**Last Sync** is the figure that matters. It tells you how much data you would lose if the source node failed right now. A job that exists but last synchronized three days ago is not protecting anything current.

---

## Step 3: Add Replication for This Machine

1. Click **Add**.
2. Select the **Target** node.
3. Set the **Schedule**.
4. Optionally set a **Rate limit**.
5. Add a **Comment**.
6. Click **Create**.

The guest is already selected, since you started from it.

The first run transfers the full disk; later runs transfer only changes. See [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md) for the full field reference.

---

### Screenshot 2

**Adding Replication From the Guest**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add replication job dialog opened from a VM's Replication tab, showing
> the target node and schedule fields with the guest already populated.

---

## Step 4: Run a Synchronization Now

Useful immediately before a planned migration, so less data has to transfer during the move.

1. Select the job.
2. Click **Schedule now**.
3. Monitor the task output.

This does not change the schedule; the next automatic run still happens as configured.

> **Verify:** Confirm the exact label of the control that triggers an immediate run.

---

### Screenshot 3

**Manual Synchronization**

```text
[ Place Screenshot Here ]
```

> **Capture:** The task output of a manually triggered replication run for a virtual
> machine.

---

## Step 5: Check the Result

1. Confirm **Last Sync** updated.
2. Confirm **Status** shows no error.
3. Confirm the duration is reasonable for the amount of change.

A duration that grows steadily over time usually means the machine is writing more than the schedule can keep up with. Either replicate less often, or accept a longer sync.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Target** | Node to replicate to. Cannot be changed on an existing job. |
| **Schedule** | How often replication runs. |
| **Rate limit** | Caps bandwidth used by the job. |
| **Enabled** | Whether the job is active. |
| **Comment** | Description of the job. |

Full reference in [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md) and [Replication Scheduling](../02-Datacenter/Replication/Replication-Scheduling.md).

> **Verify:** Capture the guest-level Add replication dialog and confirm whether its
> fields differ from the datacenter-level one.

---

# Verification

Verify the following:

* The job appears with the intended target and schedule.
* The first synchronization completed.
* **Last Sync** updates on each scheduled run.
* Duration is stable rather than growing.
* No errors in the status column.
* The target node shows the replicated data.
* Migration to the target node is faster than to an unreplicated node.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Cannot add a job | The node may not be clustered, or the machine's storage may not support replication. See [Storage Types](../02-Datacenter/Storage/Storage-Types.md). |
| Job fails on first run | Usually target storage capacity or connectivity. Check the task output. |
| Last Sync is old but status is OK | The job may be disabled. Check the Enabled state. |
| Duration growing each run | The machine changes more data than the interval accommodates. Adjust the schedule. |
| Target node cannot be changed | Correct — create a new job for the new target and remove the old one. |
| Replication stopped after migration | The job replicated *from* the old node. Review jobs after moving a machine. |
| Job missing after restore | Replication configuration is separate from the disk. Recreate it. |
| Machine cannot be replicated to two targets | It can, but not with two jobs to the same node. Create one job per target node. |

---

# Best Practices

- Check **Last Sync**, not merely whether a job exists.
- Trigger a manual run before a planned migration to shorten it.
- Match the schedule to how much data loss is acceptable — the interval is your exposure window.
- Review replication jobs after migrating a machine, since the job points at a specific source.
- Use replication **and** backup for anything important. See [Backup Jobs Overview](../02-Datacenter/Backup/Backup-Jobs-Overview.md).
- Watch duration as a trend; growth is an early warning.
- Set a rate limit where replication competes with production traffic.

---

# Related Documentation

- [Replication Overview](../02-Datacenter/Replication/Replication-Overview.md)
- [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md)
- [Replication Scheduling](../02-Datacenter/Replication/Replication-Scheduling.md)
- [Replication Status](../02-Datacenter/Replication/Replication-Status.md)
- [Replication Troubleshooting](../02-Datacenter/Replication/Replication-Troubleshooting.md)
- [Migrate Virtual Machine](Migrate-Virtual-Machine.md)
- [VM Summary](VM-Summary.md)
- [CT Replication](../05-Containers/CT-Replication.md)

---

# Summary

The guest Replication tab is a filtered view of cluster replication, scoped to one virtual machine. You can see its jobs, add one, and trigger an immediate synchronization from here — useful before a planned migration, since replicated data does not need transferring again.

The figure to read is **Last Sync**, because it measures how much data you would lose if the source node failed now. A job that exists but has not run recently is not protecting anything, and replication is not a substitute for backup regardless.
