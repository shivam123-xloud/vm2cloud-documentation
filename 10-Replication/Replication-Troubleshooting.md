# Replication Troubleshooting

---

## Overview

VM2Cloud replication provides scheduled synchronization of guest data from a source node to one or more target nodes.

Replication uses snapshots and incremental synchronization to transfer changed data after the initial synchronization. A replication failure can therefore be caused by problems with the source node, target node, storage, network, cluster communication, configuration, or the replication task itself.

This document provides a systematic procedure for identifying and resolving common replication problems.

---

## When to Use

Use this document when:

* A replication job fails.
* Replication does not start.
* Replication stops unexpectedly.
* The target node is unavailable.
* Replication takes too long.
* Replication repeatedly fails.
* Replication reports storage errors.
* Replication reports network or SSH errors.
* A replica is not up to date.
* Replication fails after migration.
* Replication configuration changes do not work as expected.

---

## Prerequisites

Before troubleshooting replication:

* You must have administrator-level access or sufficient permissions.
* Identify the affected VM or container.
* Identify the source node.
* Identify the target node.
* Identify the affected replication job.
* Have access to **Task History**.
* Confirm the cluster is available.
* Confirm the source and target nodes can be accessed.
* Avoid destructive changes until the cause has been identified.

> **Warning:** Do not delete replication data or modify storage manually while troubleshooting unless the procedure specifically requires it and the data has been verified as safe to remove.

---

# Troubleshooting Workflow

Use the following order when troubleshooting replication:

```text
Identify Failed Job
        ↓
Check Task History
        ↓
Read Error Message
        ↓
Check Replication Configuration
        ↓
Check Source Node
        ↓
Check Target Node
        ↓
Check Cluster Status
        ↓
Check Source Storage
        ↓
Check Target Storage
        ↓
Check Network Connectivity
        ↓
Check SSH Connectivity
        ↓
Check Replication Schedule
        ↓
Retry / Monitor Replication
        ↓
Verify Successful Synchronization
```

Do not change multiple configuration items at the same time unless required.

---

# Procedure

## Step 1: Identify the Affected Replication Job

1. Log in to VM2Cloud.
2. Locate the affected VM or container.
3. Select the guest.
4. Open **Replication**.
5. Identify the failed or affected replication job.
6. Record:

   * Guest ID.
   * Source node.
   * Target node.
   * Replication schedule.
   * Current status.
7. Confirm that the replication job belongs to the correct guest.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Check Task History

1. Open **Task History**.
2. Locate the most recent replication task for the guest.
3. Check the task status.
4. Check the start time.
5. Check the completion time.
6. Open the task details.
7. Read the complete task output.
8. Record the reported error.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

> **Important:** The task output is normally the first place to look when a replication operation fails.

---

## Step 3: Check the Replication Configuration

1. Return to the guest.
2. Open **Replication**.
3. Select the affected job.
4. Click **Edit**.
5. Verify the target node.
6. Verify the schedule.
7. Verify any configured bandwidth limit.
8. Confirm that the job has not been unintentionally disabled or modified.
9. Close the configuration dialog without changes if the configuration is correct.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Common Problems

## Replication Job Does Not Run

### Symptoms

The replication job exists, but no synchronization occurs.

### Possible Causes

* The scheduled time has not been reached.
* Incorrect schedule.
* Source node unavailable.
* Target node unavailable.
* Cluster problem.
* Previous replication task is still running.
* Replication configuration problem.

### Resolution

1. Open the guest's **Replication** section.
2. Verify the schedule.
3. Check the source node status.
4. Check the target node status.
5. Check cluster status.
6. Open **Task History**.
7. Check whether a previous replication task is running or failed.
8. Correct the identified problem.
9. Monitor the next scheduled replication.

---

## Replication Job Fails

### Symptoms

The replication task starts but ends with an error.

### Resolution

1. Open **Task History**.
2. Open the failed task.
3. Read the task output.
4. Identify the first meaningful error.
5. Determine whether the error is related to:

   * Storage.
   * Network.
   * SSH.
   * Target node.
   * Source node.
   * Cluster.
   * Snapshot.
   * Configuration.
6. Check the corresponding subsystem.
7. Correct the underlying issue.
8. Monitor the next scheduled replication.

---

# Source Node Problems

## Source Node Is Offline

Replication cannot proceed normally if the source guest or source node is unavailable.

### Check

1. Open the VM2Cloud node view.
2. Locate the source node.
3. Check its status.
4. Verify node connectivity.
5. Check node system health.

### Resolution

Restore the source node to normal operation and then monitor the next replication task.

---

## Source Storage Is Unavailable

### Symptoms

Replication reports storage-related errors or cannot read the guest volume.

### Check

1. Select the source node.
2. Open the storage section.
3. Check the storage status.
4. Confirm that the guest's disks are accessible.
5. Check available capacity.
6. Check for storage errors.

### Resolution

Restore storage availability before retrying replication.

---

# Target Node Problems

## Target Node Is Offline

### Symptoms

Replication cannot connect to the configured target.

### Resolution

1. Check the target node in VM2Cloud.
2. Verify that the node is online.
3. Check network connectivity.
4. Check cluster membership.
5. Check cluster status.
6. Restore target-node connectivity.
7. Monitor the next replication run.

---

## Target Storage Is Unavailable

### Symptoms

Replication starts but cannot write data to the target.

### Check

1. Select the target node.
2. Open storage information.
3. Confirm that the target storage is online.
4. Check available capacity.
5. Verify that the storage supports the required replication operation.

### Resolution

Restore the target storage and monitor the next replication run.

---

## Target Storage Is Full

### Symptoms

Replication fails because there is insufficient free space.

### Resolution

1. Check target storage usage.
2. Identify unnecessary data.
3. Check existing guest volumes.
4. Check whether the storage can be expanded.
5. Expand or free storage according to the applicable storage procedure.
6. Confirm sufficient free space.
7. Monitor the next replication.

> **Warning:** Do not delete replica data without first confirming that it is no longer required.

---

# Network Problems

## Source and Target Cannot Communicate

Replication requires communication between the source and target nodes.

### Check

1. Identify the source-node IP address.
2. Identify the target-node IP address.
3. Test network connectivity between the nodes.
4. Check routing.
5. Check firewall rules.
6. Check the replication network.
7. Verify that required node-to-node communication is allowed.

### CLI Verification

From the source node:

```bash
ping <target-node-ip>
```

From the target node:

```bash
ping <source-node-ip>
```

> Replace the placeholder IP addresses with the actual node addresses.

---

## SSH Connectivity Problem

The underlying platform uses SSH for storage replication communication.

### Check

1. Verify that the target node is reachable.
2. Verify SSH service availability.
3. Check the SSH configuration.
4. Check cluster SSH configuration.
5. Review the replication task output for SSH-related errors.

### CLI Verification

```bash
ssh <target-node>
```

If SSH connectivity fails:

1. Verify the target hostname.
2. Verify DNS or `/etc/hosts`.
3. Verify routing.
4. Verify firewall rules.
5. Verify SSH service availability.
6. Correct the underlying issue.
7. Test SSH again.

---

# Cluster Problems

## Cluster Is Not Healthy

Replication depends on correct cluster configuration and communication.

### Check

```bash
pvecm status
```

Review:

* Cluster name.
* Node count.
* Quorum.
* Node membership.
* Cluster communication state.

### Resolution

If the cluster is not healthy:

1. Resolve the cluster problem first.
2. Verify quorum.
3. Verify node membership.
4. Verify cluster communication.
5. Return to replication troubleshooting.
6. Monitor the next replication task.

---

## Cluster Has Lost Quorum

When a cluster loses quorum, configuration operations can be affected because the cluster configuration filesystem becomes read-only on a node that loses quorum.

### Check

```bash
pvecm status
```

If quorum is lost:

1. Do not make unnecessary configuration changes.
2. Identify unavailable nodes.
3. Check cluster communication.
4. Restore the required nodes or communication.
5. Confirm quorum is restored.
6. Recheck the replication job.

---

# Snapshot Problems

Replication uses snapshots to reduce the amount of data transferred during subsequent synchronization.

### Symptoms

* Replication fails during snapshot creation.
* Replication fails while removing or updating snapshots.
* Storage reports snapshot-related errors.

### Resolution

1. Check the guest storage.
2. Check whether the storage supports snapshots.
3. Check storage health.
4. Review Task History.
5. Check the exact snapshot-related error.
6. Correct the storage problem.
7. Retry replication.

---

# Replication Is Very Slow

### Possible Causes

* Large amount of changed data.
* Slow source storage.
* Slow target storage.
* Limited network bandwidth.
* Bandwidth limit configured too low.
* High network utilization.
* High storage utilization.
* Multiple replication jobs running simultaneously.

### Resolution

1. Check replication task duration.
2. Check network utilization.
3. Check source storage performance.
4. Check target storage performance.
5. Check the replication bandwidth limit.
6. Check concurrent replication jobs.
7. Review the replication schedule.
8. Adjust the configuration if required.
9. Monitor subsequent synchronization tasks.

---

# Replication Uses Too Much Network Bandwidth

### Possible Causes

* Replication schedule is too frequent.
* Large amount of data changes between synchronizations.
* Multiple jobs run simultaneously.
* No bandwidth limit is configured.

### Resolution

1. Open the replication job.
2. Click **Edit**.
3. Review the bandwidth limit.
4. Configure an appropriate limit if required.
5. Review the replication schedule.
6. Stagger multiple replication jobs when possible.
7. Save the configuration.
8. Monitor network utilization.

---

# Replication Fails After Changing the Target

### Possible Causes

* New target is unavailable.
* New target storage is unavailable.
* Target has insufficient capacity.
* Network connectivity is unavailable.
* The new target does not contain the required replica state.
* Cluster communication is unhealthy.

### Resolution

1. Open the replication configuration.
2. Confirm the new target node.
3. Check the target node status.
4. Check target storage.
5. Check available capacity.
6. Check network connectivity.
7. Check cluster status.
8. Review Task History.
9. Correct the underlying issue.
10. Monitor the next replication.

> **Important:** If the guest is migrated to a node where it is already replicated, the underlying replication mechanism can use the existing replica and automatically reverse the replication direction. If the guest is moved to a node where it is not replicated, the full disk data may need to be transferred.

---

# Replication Fails After Guest Migration

Replication behavior can change when a guest is migrated.

The underlying replication mechanism can automatically switch replication direction when a guest is migrated to an existing replication target.

### Resolution

1. Identify the guest's current node.
2. Open **Replication**.
3. Check the configured target.
4. Check Task History.
5. Confirm the replication direction.
6. Check whether the new source node can reach the target.
7. Verify target storage.
8. Monitor the next synchronization.

---

# Multiple Replication Jobs

The underlying replication framework allows a guest to be replicated to multiple target nodes, but not twice to the same target node.

If multiple jobs are configured:

1. Open the guest's **Replication** section.
2. Review every replication job.
3. Check each target node.
4. Confirm that targets are unique.
5. Review schedules.
6. Check network and storage utilization.
7. Verify each job independently.

---

# Replication and High Availability

High Availability can be used together with storage replication, but replication does not provide zero data loss.

There may be data loss between the most recent successful synchronization and the time of a node failure.

### Troubleshooting Procedure

If an HA-managed guest has a replication problem:

1. Check HA status.
2. Check cluster quorum.
3. Check source node.
4. Check target node.
5. Check replication status.
6. Check Task History.
7. Check storage.
8. Check network connectivity.
9. Resolve the underlying issue.
10. Verify subsequent replication.

> **Warning:** Do not assume that a successful HA recovery means that every change made after the last successful replication was preserved.

---

# Replication Keeps Failing Repeatedly

If the same replication job fails repeatedly:

1. Record the guest ID.
2. Record the source node.
3. Record the target node.
4. Record the replication schedule.
5. Record the exact error message.
6. Review several failed task outputs.
7. Check storage health.
8. Check network connectivity.
9. Check SSH connectivity.
10. Check cluster status.
11. Check source and target node availability.
12. Check target storage capacity.
13. Compare failures against the replication schedule.
14. Correct the underlying issue.
15. Run or wait for the next scheduled synchronization.
16. Verify successful completion.

Do not repeatedly delete and recreate the replication job without identifying the underlying problem.

---

# Verification After Troubleshooting

After correcting a replication problem:

1. Open the guest.
2. Open **Replication**.
3. Verify the replication job.
4. Verify the target node.
5. Verify the schedule.
6. Confirm the source node is available.
7. Confirm the target node is available.
8. Confirm source storage availability.
9. Confirm target storage availability.
10. Confirm cluster health.
11. Monitor the next replication task.
12. Open **Task History**.
13. Confirm successful completion.
14. Monitor at least one additional scheduled synchronization when the workload is critical.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# CLI Verification

CLI is secondary and should be used for verification and troubleshooting when the UI does not provide sufficient information.

## Check Replication Status

```bash
pvesr status
```

The underlying `pvesr` utility manages the storage replication framework.

---

## Check Cluster Status

```bash
pvecm status
```

Use this to check cluster membership and quorum.

---

## Check Network Connectivity

```bash
ping <target-node-ip>
```

---

## Check SSH Connectivity

```bash
ssh <target-node>
```

---

## Check Node Services

If service-level troubleshooting is required:

```bash
systemctl status pve-cluster
systemctl status corosync
```

Use service commands only when investigating a node or cluster problem.

---

# Troubleshooting Checklist

Use this checklist before escalating a replication problem:

```text
[ ] Correct guest identified
[ ] Correct replication job identified
[ ] Correct source node identified
[ ] Correct target node identified
[ ] Replication schedule verified
[ ] Source node online
[ ] Target node online
[ ] Cluster has quorum
[ ] Cluster communication working
[ ] Source storage available
[ ] Target storage available
[ ] Target storage has sufficient capacity
[ ] Source and target can communicate
[ ] SSH connectivity verified
[ ] Replication task output reviewed
[ ] Snapshot-related errors checked
[ ] Bandwidth limit reviewed
[ ] Replication duration reviewed
[ ] No conflicting replication jobs
[ ] Next replication completed successfully
```

---

# Safety Considerations

> **Warning:** Do not delete replication data as a first troubleshooting step.

> **Warning:** Do not manually modify replication configuration files unless the official recovery procedure requires it.

> **Warning:** Do not disable cluster or storage services without understanding the effect on running workloads.

> **Warning:** Do not treat replication as a replacement for independent backups.

> **Warning:** Replication can still result in data loss between the last successful synchronization and a node failure.

Before performing destructive troubleshooting:

1. Verify the affected guest.
2. Verify the latest successful replication.
3. Verify backup availability.
4. Confirm the recovery requirements.
5. Obtain appropriate administrative approval when required.
6. Perform the smallest corrective action necessary.
7. Verify the result.

---

# Best Practices

* Always start troubleshooting from Task History.
* Record the exact replication error.
* Check the source and target nodes.
* Check cluster quorum.
* Check source and target storage.
* Check network connectivity.
* Check SSH connectivity when required.
* Monitor storage capacity.
* Review replication schedules.
* Avoid unnecessarily short replication intervals.
* Avoid excessive bandwidth limits that prevent synchronization from completing.
* Do not repeatedly recreate jobs without identifying the cause.
* Maintain independent backups.
* Test recovery procedures regularly.
* Verify successful synchronization after every corrective change.
* Document recurring replication failures and their resolution.

---

# Related Documentation

```text
10-Replication/
├── Replication-Overview.md
├── Create-Replication-Job.md
├── Edit-Replication-Job.md
├── Delete-Replication-Job.md
├── Replication-Scheduling.md
├── Replication-Status.md
└── Replication-Troubleshooting.md
```

Related cluster documentation:

```text
17-Cluster-Management/
├── Cluster-Overview.md
├── Cluster-Status.md
├── Quorum.md
├── Corosync.md
└── Cluster-Troubleshooting.md
```

Related storage documentation:

```text
06-Storage/
├── Storage-Overview.md
├── Storage-Content.md
└── Storage-Troubleshooting.md
```

Related backup documentation:

```text
08-Backup-and-Restore/
├── Backup-Overview.md
├── Backup-Verification.md
└── Backup-Troubleshooting.md
```

Related HA documentation:

```text
09-High-Availability/
├── HA-Overview.md
├── HA-Resources.md
├── Quorum.md
└── HA-Troubleshooting.md
```

---

# Summary

Replication troubleshooting should follow a structured process rather than repeatedly restarting or recreating replication jobs.

The recommended workflow is:

```text
Identify Job
    ↓
Check Task History
    ↓
Read Error
    ↓
Check Configuration
    ↓
Check Source Node
    ↓
Check Target Node
    ↓
Check Cluster
    ↓
Check Storage
    ↓
Check Network
    ↓
Check SSH
    ↓
Correct Problem
    ↓
Run Next Synchronization
    ↓
Verify Successful Replication
```

The most important troubleshooting information is normally the replication task output, because it identifies the specific operation that failed.

VM2Cloud replication uses the underlying storage-replication framework, which performs incremental synchronization after the initial synchronization and uses snapshots to reduce transferred data.

Successful replication must be verified through actual completed synchronization tasks. The existence of a configured replication job alone does not prove that the replica is current.
