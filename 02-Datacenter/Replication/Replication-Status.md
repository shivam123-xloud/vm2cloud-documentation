# Replication Status

---

## Overview

VM2Cloud VE provides replication status information for configured replication jobs.

The replication status allows administrators to determine whether a replication job is:

* Configured correctly.
* Running.
* Waiting for its next scheduled synchronization.
* Successfully synchronized.
* Experiencing an error.
* Unable to synchronize because of a node, storage, network, or cluster problem.

Replication status should be checked regularly for production workloads to ensure that the target contains a recent replica.

---

## When to Use

Use replication status to:

* Verify that a replication job is working.
* Check the latest synchronization.
* Identify failed replication tasks.
* Check the target node.
* Monitor replication after creating a job.
* Monitor replication after changing a job.
* Investigate replication failures.
* Confirm that scheduled replication is occurring.
* Verify replication before maintenance or recovery operations.

---

## Prerequisites

Before checking replication status:

* You must be logged in to VM2Cloud VE.
* You must have permission to view the guest and replication configuration.
* The guest must exist.
* A replication job should be configured for the guest.
* The source node should be accessible.
* The cluster should be operating normally.

---

# Procedure

## Step 1: Select the Guest

1. Log in to VM2Cloud VE.
2. Locate the required VM or container in the navigation tree.
3. Select the guest.
4. Open the **Replication** section.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Replication Jobs

1. Review the replication jobs listed for the selected guest.
2. Identify the required replication job.
3. Check the configured target node.
4. Check the configured schedule.
5. Check the displayed replication status information.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Identify the Current Replication State

Review the information displayed for the replication job.

Depending on the installed VM2Cloud VE version and current job state, the interface may provide information such as:

* Replication target.
* Replication schedule.
* Last synchronization.
* Next scheduled synchronization.
* Replication state.
* Task status.
* Error information.

Use the displayed information to determine whether the replication job is operating normally.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Understanding Replication Status

## Healthy Replication

A replication job is considered healthy when:

* The job exists.
* The target node is available.
* Scheduled synchronization occurs.
* The latest synchronization completes successfully.
* No replication errors are reported.
* The replica is reasonably current for the configured schedule.

---

## Replication Running

A replication task may be actively synchronizing data.

During synchronization:

1. The source guest remains available according to normal VM2Cloud VE operation.
2. Changed guest data is synchronized to the target.
3. The replication task can be monitored through task information.
4. Wait for the task to complete before judging the final synchronization result.

Do not interrupt a replication task unless there is a specific operational reason to do so.

---

## Replication Waiting

A job can be waiting for its next scheduled synchronization.

This is normal when:

* The previous synchronization completed successfully.
* The next scheduled run has not occurred yet.
* The replication job is enabled.

Check the configured schedule before treating a waiting state as a problem.

---

## Replication Failed

A failed replication indicates that the scheduled synchronization did not complete successfully.

Possible causes include:

* Source node problem.
* Target node problem.
* Storage problem.
* Network connectivity problem.
* Insufficient target storage.
* Cluster problem.
* Replication configuration problem.
* Previous task failure.

When a failure occurs, check the associated task output and investigate the underlying cause.

---

# Check Task History

Task History provides additional information about replication operations.

## Step 1: Open Task History

1. Open **Task History** from the appropriate VM2Cloud VE interface location.
2. Review the most recent tasks.
3. Locate the replication task for the guest.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Step 2: Identify the Replication Task

1. Locate the task associated with the required guest.
2. Check the task start time.
3. Check the task completion time.
4. Check the task status.
5. Open the task output when additional information is required.

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Step 3: Review Task Output

If the task failed:

1. Open the task details.
2. Read the task output.
3. Identify the reported error.
4. Determine whether the error relates to:

   * Network connectivity.
   * Storage.
   * Target node.
   * Source node.
   * Cluster communication.
   * Replication configuration.
5. Correct the underlying problem.
6. Monitor the next replication run.

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Verify the Target Node

The replication job configuration identifies the target node.

To verify it:

1. Open the guest.
2. Open **Replication**.
3. Select the replication job.
4. Check the target node.
5. Confirm that the target node is online.
6. Confirm that the target node belongs to the cluster.
7. Check target storage availability.

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

# Verify the Replication Schedule

A replication job only runs according to its configured schedule.

To verify the schedule:

1. Select the replication job.
2. Click **Edit**.
3. Review the **Schedule** field.
4. Confirm that the schedule is correct.
5. Close the dialog without making changes if no modification is required.

### Screenshot 8

```text
[ Place Screenshot Here ]
```

For schedule configuration details, see:

`10-Replication/Replication-Scheduling.md`

---

# Verify Replication After Creating a Job

After creating a new replication job:

1. Open the guest.
2. Open **Replication**.
3. Confirm that the new job is listed.
4. Verify the target node.
5. Verify the schedule.
6. Wait for the scheduled synchronization.
7. Open **Task History**.
8. Locate the replication task.
9. Confirm successful completion.
10. Continue monitoring subsequent synchronizations.

### Screenshot 9

```text
[ Place Screenshot Here ]
```

---

# Verify Replication After Editing a Job

After changing a replication job:

1. Open the guest.
2. Open **Replication**.
3. Verify the modified configuration.
4. Confirm the target node.
5. Confirm the schedule.
6. Wait for the next synchronization.
7. Open **Task History**.
8. Review the resulting replication task.
9. Confirm successful synchronization.

---

# Verify Replication After Changing the Target

Changing the replication target requires additional verification.

1. Open the guest.
2. Open **Replication**.
3. Confirm the new target node.
4. Verify that the target node is online.
5. Verify target storage availability.
6. Monitor the next synchronization.
7. Open Task History.
8. Confirm that synchronization completes successfully.
9. Monitor subsequent replication runs.

> **Warning:** A target-node change can require synchronization of data to the new target. Verify that sufficient storage and network capacity are available.

---

# Common Issues

## No Replication Job Is Listed

Possible causes:

* No replication job exists.
* The wrong guest was selected.
* The replication job was deleted.
* The guest configuration is unavailable.
* The user does not have sufficient permissions.

### Resolution

1. Confirm the correct guest.
2. Open **Replication**.
3. Check whether a replication job exists.
4. Check the user's permissions.
5. If required, create a new replication job.

---

## Replication Has Not Run

Possible causes:

* The scheduled time has not been reached.
* The schedule is incorrect.
* The source node is unavailable.
* The target node is unavailable.
* The replication job has a configuration problem.
* A previous task is still running.

### Resolution

1. Check the replication schedule.
2. Check the source node.
3. Check the target node.
4. Check cluster status.
5. Open Task History.
6. Review previous replication tasks.
7. Correct the reported problem.
8. Monitor the next scheduled run.

---

## Last Replication Failed

Possible causes:

* Network connectivity failure.
* Target storage unavailable.
* Insufficient storage capacity.
* Target node offline.
* Source storage problem.
* Cluster communication problem.

### Resolution

1. Open Task History.
2. Open the failed replication task.
3. Read the task output.
4. Check source-node status.
5. Check target-node status.
6. Check source storage.
7. Check target storage.
8. Check network connectivity.
9. Check cluster status.
10. Correct the underlying problem.
11. Monitor the next synchronization.

---

## Replication Is Behind Schedule

Possible causes:

* Large amount of changed data.
* Slow storage.
* Limited network bandwidth.
* Too many replication jobs.
* Target-node performance issue.
* Replication interval is too short for the workload.

### Resolution

1. Check replication task duration.
2. Check network utilization.
3. Check source storage performance.
4. Check target storage performance.
5. Check concurrent replication jobs.
6. Review the configured schedule.
7. Adjust the schedule or bandwidth limit if required.
8. Monitor subsequent synchronizations.

---

## Target Node Is Offline

Possible causes:

* Node shutdown.
* Network failure.
* Hardware failure.
* Cluster communication problem.

### Resolution

1. Check the target node in the VM2Cloud VE interface.
2. Verify node availability.
3. Check cluster status.
4. Check network connectivity.
5. Restore target-node availability.
6. Monitor the next replication task.

---

## Target Storage Is Full

If the target storage does not have sufficient free space:

1. Check target storage utilization.
2. Identify unnecessary data.
3. Review existing replicas.
4. Review storage allocation.
5. Free or extend storage according to the applicable storage procedure.
6. Monitor the next replication run.

> **Warning:** Do not delete replica data blindly. Confirm that the data is not required for recovery.

---

# CLI Verification

CLI is secondary and should be used for verification or troubleshooting when the UI does not provide sufficient information.

The underlying replication management utility is `pvesr`.

To check replication status:

```bash
pvesr status
```

The command can be used to obtain replication information from the underlying platform. The official administration guide documents `pvesr` as the storage replication management utility.

To check the scheduler daemon:

```bash
pvescheduler status
```

The underlying scheduler is responsible for starting scheduled jobs, including replication jobs.

To check cluster status:

```bash
pvecm status
```

Use CLI output together with the VM2Cloud VE UI when troubleshooting.

---

# Verification Checklist

Use the following checklist when verifying a replication job:

```text
[ ] Correct guest selected
[ ] Replication job exists
[ ] Correct target node configured
[ ] Target node online
[ ] Source node online
[ ] Cluster operating normally
[ ] Source storage available
[ ] Target storage available
[ ] Target storage has sufficient free space
[ ] Correct replication schedule
[ ] Latest replication task completed successfully
[ ] No current replication errors
[ ] Next synchronization is scheduled
[ ] Backup strategy is available
```

---

# Best Practices

* Check replication status regularly.
* Do not rely only on the existence of a replication job.
* Verify that synchronization actually completes successfully.
* Review Task History after failures.
* Monitor target storage capacity.
* Monitor network utilization.
* Verify target-node availability.
* Check cluster health when replication fails.
* Verify replication after configuration changes.
* Maintain independent backups.
* Test recovery procedures regularly.
* Investigate repeated replication failures instead of repeatedly retrying without identifying the cause.

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

Cluster:

- [Cluster Overview](../Cluster/Cluster-Overview.md)
- [Quorum](../Cluster/Quorum.md)
- [Cluster Troubleshooting](../Cluster/Cluster-Troubleshooting.md)

Storage:

- [Storage Overview](../Storage/Storage-Overview.md)
- [Storage Troubleshooting](../Storage/Storage-Troubleshooting.md)

---

# Summary

Replication Status is used to verify that VM2Cloud VE replication jobs are configured and synchronizing successfully.

The normal verification workflow is:

```text
Select Guest
    ↓
Open Replication
    ↓
Review Replication Job
    ↓
Check Target
    ↓
Check Schedule
    ↓
Review Task History
    ↓
Check Latest Synchronization
    ↓
Investigate Errors if Required
```

A configured replication job should not be considered healthy merely because it appears in the Replication section. The latest synchronization should be checked and successful task completion should be verified.

For repeated failures, investigate the source node, target node, storage, network, cluster state, and replication configuration before making further changes.
