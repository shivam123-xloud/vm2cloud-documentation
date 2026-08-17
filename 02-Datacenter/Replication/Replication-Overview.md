# Replication Overview

---

## Overview

VM2Cloud VE Storage Replication provides a way to replicate virtual machine and container data from one cluster node to another node.

Replication is designed primarily for guests using local storage. It maintains a copy of the guest's data on another cluster node without requiring shared storage.

Replication uses snapshots to transfer only changed data after the initial synchronization. This reduces the amount of data transferred during subsequent replication runs.

Replication can help reduce recovery and migration time when a guest already has a synchronized copy on the target node.

Replication is **not a replacement for backup**. Replication maintains copies of guest data on another node, while backups provide independent recovery points.

---

## When to Use

Use replication when:

* A VM or container uses supported local storage.
* A copy of guest data should be maintained on another cluster node.
* Faster recovery after a node failure is required.
* Shared storage is not available.
* Migration between nodes should require less data transfer.
* Guest data needs to be synchronized periodically to another node.
* A guest needs replication to more than one target node.

Replication can be configured for multiple target nodes, but the same guest cannot have two replication jobs targeting the same node.

---

## Prerequisites

Before configuring replication:

* VM2Cloud VE must have a working cluster.
* The source node and target node must be cluster members.
* The guest must exist on the source node.
* The guest's storage must support replication.
* The target node must have compatible storage.
* The source and target nodes must have reliable network connectivity.
* The cluster must have quorum.
* The required storage must have sufficient free capacity.
* The administrator must have sufficient permissions to manage replication.
* For production workloads, maintain a separate backup strategy.

### Supported Storage

The underlying replication framework supports specific storage types. In the documented stable configuration, local ZFS storage is supported for storage replication.

Verify the storage type in the actual VM2Cloud VE environment before creating a replication job.

---

# Procedure

## Step 1: Open Replication

1. Log in to VM2Cloud VE.
2. Select **Datacenter** from the left navigation tree.
3. Open **Replication**.
4. Review the existing replication jobs.
5. Check whether the required guest already has a replication job.

### Screenshot 1

**Datacenter Replication Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Replication, showing every job in the cluster.

The Replication view can also be accessed at node or guest level. The displayed jobs depend on the selected level.

---

## Step 2: Review Existing Replication Jobs

Review the replication list before creating a new job.

Check:

* Guest.
* Target node.
* Job ID.
* Schedule.
* Replication status.
* Last successful synchronization.
* Any configured bandwidth limitation.
* Whether the job is enabled.

### Screenshot 2

**Job Columns**

```text
[ Place Screenshot Here ]
```

> **Capture:** The list showing schedule, status, last successful sync, rate limit, and
> enabled state.

---

## Step 3: Identify the Guest

Before creating a replication job, identify:

1. The VM or container ID.
2. The current source node.
3. The storage containing the guest data.
4. The target node.
5. The target storage requirements.
6. The required replication interval.

Replication jobs are associated with a specific guest and target node.

---

## Step 4: Create a Replication Job

1. Open the **Replication** panel.
2. Click **Add**.
3. Select the guest if it was not already selected.
4. Select the target node.
5. Configure the replication schedule.
6. Configure a bandwidth limit if required.
7. Review the configuration.
8. Click **Create** or the corresponding confirmation button shown by the installed VM2Cloud VE version.
9. Verify that the new replication job appears in the replication list.

### Screenshot 3

**Create Dialog From Datacenter Level**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create dialog opened here rather than from a guest, so the guest
> selector is visible.

The exact button labels can vary between VM2Cloud VE releases because VM2Cloud VE follows the underlying platform version.

---

# Configuration / Options

## Guest

The **Guest** identifies the VM or container whose storage data will be replicated.

The guest must be located on a storage type supported by the replication framework.

---

## Target Node

The **Target Node** identifies where the guest's replicated data will be maintained.

Choose a node that:

* Is a member of the same cluster.
* Is reachable from the source node.
* Has compatible supported storage.
* Has sufficient free capacity.
* Is suitable for the intended recovery or migration scenario.

---

## Schedule

The **Schedule** determines when replication runs.

The underlying replication framework supports configurable replication intervals. The minimum documented interval is one minute and the maximum is one week.

Choose an interval based on:

* Guest write activity.
* Network capacity.
* Storage performance.
* Required recovery point objective.
* Number of replication jobs.

A shorter interval can reduce the amount of unsynchronized data after a failure, but it can increase storage and network activity.

---

## Bandwidth Limit

A replication job can have a bandwidth limit.

Use a bandwidth limit when replication traffic could negatively affect:

* Production VM traffic.
* Container traffic.
* Storage traffic.
* Cluster communication.
* Other replication jobs.

The underlying replication framework supports per-job bandwidth limiting.

---

## Job ID

Each replication job has a unique cluster-wide job identifier.

The underlying platform uses the guest ID together with a job number to identify the replication job.

The job ID is useful when:

* Reviewing replication status.
* Troubleshooting.
* Identifying a specific replication task.
* Performing CLI-based administration.

---

# How Replication Works

Replication normally occurs in two stages.

## Initial Synchronization

During the first replication:

1. The guest's required data is examined.
2. The initial data set is transferred to the target node.
3. The target receives the replicated guest data.
4. The initial synchronization completes.

The initial synchronization can require significant network bandwidth and time depending on guest disk size and storage performance.

---

## Incremental Synchronization

After the initial synchronization:

1. VM2Cloud VE creates or uses snapshots to identify changes.
2. Only changed data is transferred.
3. The target copy is updated.
4. The replication job completes.
5. The process repeats according to the configured schedule.

This incremental behavior reduces network traffic compared with repeatedly transferring the complete guest disk.

---

# Replication and Migration

Replication can reduce migration time when the guest is migrated to a node that already contains a current replica.

For example:

```text
Node A
  |
  | Replication
  v
Node B
```

If the guest is later migrated from Node A to Node B, only changes that occurred after the last synchronization may need to be transferred.

If the guest is migrated to a node that does not already contain a replica, the complete disk data may need to be transferred.

---

# Replication Direction After Migration

Replication follows the guest's current location.

Example:

```text
Before migration:

Node A
  |
  | Replication
  v
Node B
```

If the guest is migrated to Node B:

```text
Node B
  |
  | Replication
  v
Node A
```

The underlying replication framework automatically changes the replication direction after the guest is migrated to its replication target.

---

# Replication and High Availability

Replication can be used together with High Availability.

However, replication does not guarantee zero data loss.

If a source node fails before the most recent replication completes, changes made after the last successful synchronization may not exist on the target node.

Therefore:

```text
Replication
    ≠
Backup
```

and:

```text
Replication
    ≠
Zero Data Loss
```

The underlying documentation explicitly notes that HA can be used with storage replication, but some data may be lost between the last successful synchronization and a node failure.

---

# Replication and Backup

Replication and backup solve different problems.

| Feature                             | Replication                   | Backup                       |
| ----------------------------------- | ----------------------------- | ---------------------------- |
| Main purpose                        | Maintain a current copy       | Provide recovery points      |
| Target                              | Cluster node                  | Backup storage               |
| Synchronization                     | Periodic                      | Scheduled                    |
| Historical recovery points          | Limited by replication design | Multiple backup points       |
| Protection from source-node failure | Yes                           | Yes                          |
| Protection from accidental deletion | Limited                       | Better                       |
| Protection from ransomware          | Limited                       | Better with isolated backups |
| Independent recovery copy           | No                            | Yes                          |
| Replacement for backup              | No                            | N/A                          |

A production VM2Cloud VE environment should normally use both replication and backup where appropriate.

---

# Replication to Multiple Nodes

A guest can be replicated to multiple target nodes.

For example:

```text
             ┌──> Node B
Node A ──────┤
             └──> Node C
```

However, the same guest cannot have two separate replication jobs targeting the same target node.

Before creating multiple jobs:

1. Identify the source node.
2. Identify each target node.
3. Confirm that each target is different.
4. Confirm storage availability.
5. Confirm network capacity.
6. Configure the required schedules.
7. Monitor each replication job separately.

---

# Verification

After creating a replication job:

1. Open **Datacenter → Replication**.
2. Locate the new job.
3. Verify the guest.
4. Verify the target node.
5. Verify the schedule.
6. Verify that the job is enabled.
7. Wait for the scheduled synchronization.
8. Review the replication status.
9. Confirm that the first synchronization completes successfully.
10. Verify that the target contains the replicated data.

### Screenshot 4

**Job Running Normally**

```text
[ Place Screenshot Here ]
```

> **Capture:** A job with a recent successful sync and a next run scheduled.

---

## Verify the Last Synchronization

Check the replication job status and identify:

* Last run.
* Last successful synchronization.
* Current state.
* Error state, if any.

A successful replication job should show that synchronization completed without errors.

---

## Verify Target Storage

1. Select the target node.
2. Open the relevant storage.
3. Review the available content.
4. Confirm that the replicated guest data exists.
5. Compare available storage capacity before and after replication.

### Screenshot 5

**Replicated Data on the Target**

```text
[ Place Screenshot Here ]
```

> **Capture:** The target node's storage content listing the replicated volume.

---

# Common Issues

## Replication Job Does Not Start

Possible causes:

* Guest is not on supported storage.
* Target node is unavailable.
* Cluster has lost quorum.
* Storage is unavailable.
* Network connectivity is unavailable.
* Replication job is disabled.
* Another replication operation is already running.

Check:

1. Guest status.
2. Source node.
3. Target node.
4. Storage status.
5. Cluster quorum.
6. Replication job configuration.
7. Task history.

---

## Initial Replication Takes Too Long

Possible causes:

* Large guest disk.
* Slow source storage.
* Slow target storage.
* Limited network bandwidth.
* High network utilization.
* High storage activity.

Recommended actions:

1. Check storage performance.
2. Check network utilization.
3. Check the configured bandwidth limit.
4. Schedule initial replication during a suitable maintenance period.
5. Avoid unnecessary concurrent replication jobs.

---

## Replication Fails Because Storage Is Unsupported

Replication requires supported storage.

The underlying documented stable storage type for storage replication is local ZFS.

Resolution:

1. Identify the guest's storage.
2. Check the storage type.
3. Confirm whether it supports replication.
4. Move the guest storage to supported storage if appropriate.
5. Reconfigure the replication job.
6. Run the replication again.

---

## Replication Target Has Insufficient Space

Possible causes:

* Target storage is almost full.
* Guest disk is larger than expected.
* Other guests consume the target storage.
* Replicated snapshots consume space.

Resolution:

1. Check target storage usage.
2. Identify unnecessary data.
3. Free capacity where appropriate.
4. Expand the target storage if required.
5. Re-run replication.

> **Warning:** Do not delete replicated data without understanding which replication job depends on it.

---

## Replication Network Is Slow

Possible causes:

* Network congestion.
* Bandwidth limitation.
* High storage activity.
* Poor network path.
* Packet loss.
* Incorrect network configuration.

Resolution:

1. Check network utilization.
2. Check the replication network.
3. Check the bandwidth limit.
4. Check node connectivity.
5. Check packet loss.
6. Check network interfaces.
7. Correct the network problem.
8. Monitor the next replication run.

---

## Replication Job Reports an Error

1. Open **Datacenter → Replication**.
2. Locate the affected job.
3. Review its status.
4. Open the related task.
5. Read the task output.
6. Identify the failing component.
7. Check source storage.
8. Check target storage.
9. Check source and target nodes.
10. Check network connectivity.
11. Correct the underlying problem.
12. Verify the next replication run.

### Screenshot 6

**Job Reporting an Error**

```text
[ Place Screenshot Here ]
```

> **Capture:** A job row in an error state. Skip this if nothing has failed — do not
> stage a failure for it.

---

# CLI Verification

CLI is secondary and should normally be used only for verification or troubleshooting.

The underlying replication framework is managed by the `pvesr` utility.

To inspect available replication information:

```bash
pvesr status
```

Use CLI output together with the VM2Cloud VE web UI when troubleshooting.

Do not use CLI to replace an available UI workflow unless the UI cannot perform the required operation.

---

# Important Limitations

Replication should not be treated as a complete disaster-recovery solution.

Consider the following:

* Replication is periodic rather than continuous.
* Changes made after the last successful synchronization may be lost after a node failure.
* Replication requires supported storage.
* The target node must have sufficient capacity.
* Network bandwidth affects synchronization time.
* Replication does not provide the same historical recovery capabilities as backup.
* Replicated data should not be the only copy of production data.

---

# Best Practices

* Use replication for workloads that require faster recovery.
* Use supported storage for replication.
* Maintain reliable cluster networking.
* Monitor replication status regularly.
* Configure schedules according to workload requirements.
* Avoid unnecessarily aggressive schedules.
* Use bandwidth limits when required.
* Ensure target storage has sufficient free capacity.
* Keep separate backups of replicated workloads.
* Test recovery procedures.
* Monitor failed replication jobs.
* Investigate repeated replication failures.
* Document which workloads are replicated and where.
* Do not rely on replication as the only disaster-recovery mechanism.

---

# Related Documentation

Replication:

- [Replication Overview](Replication-Overview.md)
- [Create Replication Job](Create-Replication-Job.md)
- [Edit Replication Job](Edit-Replication-Job.md)
- [Delete Replication Job](Delete-Replication-Job.md)
- [Replication Scheduling](Replication-Scheduling.md)
- [Replication Status](Replication-Status.md)
- [Replication Troubleshooting](Replication-Troubleshooting.md)

Cluster and HA:

- [Cluster Overview](../Cluster/Cluster-Overview.md)
- [Quorum](../Cluster/Quorum.md)
- [HA Overview](../HA/HA-Overview.md)

Backup:

- [Backup and Restore Virtual Machine](../../04-Virtual-Machines/Backup-and-Restore-VM.md)
- [Backup and Restore Container](../../05-Containers/Backup-and-Restore-Container.md)

Storage:

- [Storage Overview](../Storage/Storage-Overview.md)
- [Storage Types](../Storage/Storage-Types.md)

---

# Summary

VM2Cloud VE Storage Replication maintains synchronized copies of supported guest storage on another cluster node.

The main characteristics are:

* Replication uses snapshots.
* The initial synchronization transfers the initial data set.
* Later synchronizations transfer changes incrementally.
* Replication can reduce migration time.
* Replication can target multiple different nodes.
* Replication can work together with HA.
* Replication does not replace backup.
* Data changed after the last successful synchronization may be lost after a node failure.
* Storage and network availability are critical to successful replication.

For production environments, use replication together with an appropriate backup and recovery strategy.
