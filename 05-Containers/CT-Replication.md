# Container Replication

---

## Overview

The **Replication** tab on a container shows the replication jobs configured for that container, and lets you create, edit, and run them from the container itself.

It is a filtered view of cluster replication, scoped to one container. Jobs created here appear in the datacenter list and vice versa.

For what replication is, how it differs from backup, and the full job configuration, see [Replication Overview](../02-Datacenter/Replication/Replication-Overview.md). For the virtual machine equivalent of this panel, see [VM Replication](../04-Virtual-Machines/VM-Replication.md) — the two behave the same way.

> **Note:** Replication requires a cluster and supported local storage. On a standalone node the tab is present but nothing can be configured.

---

## When to Use

Use the container Replication tab when you need to:

* Check whether this container is replicated, and to where.
* See when it last synchronized successfully.
* Add replication for this container specifically.
* Trigger a synchronization before a planned migration.
* Investigate a replication failure affecting this container.

---

## Prerequisites

* You have permission to modify the container.
* The node is part of a cluster with quorum.
* The container's root disk is on storage that supports replication.
* The target node has compatible storage with sufficient capacity.

---

# Procedure

## Step 1: Open the Replication Tab

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select the container.
4. Click **Replication**.

---

### Screenshot 1

**Container Replication Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A container → Replication, showing a configured job with target, schedule,
> last sync, duration, and status, plus the **Add**, **Edit**, **Remove**, and
> **Schedule now** controls.

---

## Step 2: Read the Job List

| Column | Meaning |
|---|---|
| **Target** | The node the container replicates to. |
| **Schedule** | How often the job runs. |
| **Last Sync** | When it last completed successfully. |
| **Next Sync** | When it will run again. |
| **Duration** | How long the last run took. |
| **Status** | `OK`, running, or an error. |

**Last Sync** is the number that matters — it is how much data you would lose if the source node failed now.

---

## Step 3: Add Replication for This Container

1. Click **Add**.
2. Select the **Target** node.
3. Set the **Schedule**.
4. Optionally set a **Rate limit**.
5. Add a **Comment**.
6. Click **Create**.

The container is already selected. The first run transfers the full root disk; later runs transfer only changes.

Mount points are worth checking. A container with additional mount points replicates those only if they are on replication-capable storage — a mount point on unsupported storage is not covered, and the container will be incomplete on the target.

> **Verify:** Confirm how mount points on non-replicating storage are reported when
> creating a replication job for a container.

---

### Screenshot 2

**Adding Replication From the Container**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add replication job dialog opened from a container's Replication tab.

---

## Step 4: Run a Synchronization Now

1. Select the job.
2. Click **Schedule now**.
3. Monitor the task output.

This does not affect the schedule.

---

## Step 5: Check the Result

1. Confirm **Last Sync** updated.
2. Confirm **Status** shows no error.
3. Confirm the duration is reasonable.

---

### Screenshot 3

**Manual Synchronization**

```text
[ Place Screenshot Here ]
```

> **Capture:** The task output of a manually triggered replication run for a container.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Target** | Node to replicate to. Cannot be changed on an existing job. |
| **Schedule** | How often replication runs. |
| **Rate limit** | Caps bandwidth used by the job. |
| **Enabled** | Whether the job is active. |
| **Comment** | Description of the job. |

Full reference in [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md).

---

# Verification

Verify the following:

* The job appears with the intended target and schedule.
* The first synchronization completed.
* **Last Sync** updates on each run.
* All mount points that should be replicated are included.
* Duration is stable rather than growing.
* No errors in the status column.
* Migration to the target node is faster than to an unreplicated node.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Cannot add a job | The node may not be clustered, or the root disk may be on storage that does not support replication. |
| Mount point missing on the target | That mount point is on storage that does not replicate. Move it or accept the gap. |
| Job fails on first run | Usually target capacity or connectivity. Check the task output. |
| Last Sync is old but status is OK | The job may be disabled. |
| Duration growing each run | The container writes more than the interval accommodates. Adjust the schedule. |
| Target cannot be changed | Correct — create a new job and remove the old one. |
| Replication stopped after migration | The job replicated from the previous node. Review jobs after moving a container. |
| Job missing after restore | Replication configuration is separate from the container. Recreate it. |

---

# Best Practices

- Check **Last Sync** rather than merely confirming a job exists.
- Verify that every mount point you rely on is actually being replicated.
- Trigger a manual run before a planned migration.
- Match the schedule to the data loss you can tolerate.
- Review jobs after migrating a container.
- Use replication **and** backup for anything important.
- Set a rate limit where replication competes with production traffic.

---

# Related Documentation

- [Replication Overview](../02-Datacenter/Replication/Replication-Overview.md)
- [Create Replication Job](../02-Datacenter/Replication/Create-Replication-Job.md)
- [Replication Scheduling](../02-Datacenter/Replication/Replication-Scheduling.md)
- [Replication Status](../02-Datacenter/Replication/Replication-Status.md)
- [Replication Troubleshooting](../02-Datacenter/Replication/Replication-Troubleshooting.md)
- [Migrate Container](Migrate-Container.md)
- [Manage Container Resources](Manage-Container-Resources.md)
- [CT Summary](CT-Summary.md)
- [VM Replication](../04-Virtual-Machines/VM-Replication.md)

---

# Summary

The container Replication tab is a filtered view of cluster replication scoped to one container, letting you see its jobs, add one, and trigger an immediate synchronization.

Two things deserve attention. **Last Sync** measures your actual exposure, not the existence of a job. And a container's additional mount points replicate only if they sit on replication-capable storage — check that, or the copy on the target will be missing data you assumed was covered.
