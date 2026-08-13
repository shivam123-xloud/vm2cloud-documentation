# Replication Scheduling

---

## Overview

VM2Cloud VE replication jobs run automatically according to a configured schedule.

The replication scheduler determines when a replication job starts synchronization between the source node and the target node.

The underlying replication scheduler supports configurable intervals from **one minute to one week**. Replication schedules use a subset of the **systemd calendar-event format**.

The schedule controls when synchronization starts. It does not continuously replicate every change as it occurs.

---

## When to Use

Use replication scheduling when:

* Replication must run automatically.
* Different workloads require different synchronization intervals.
* Network bandwidth must be controlled.
* Storage performance must be considered.
* A smaller recovery-point gap is required.
* Replication should run during specific periods.
* Replication traffic should be reduced during peak production hours.

---

## Prerequisites

Before configuring a replication schedule:

* A replication job must already exist.
* The source and target nodes must be cluster members.
* The guest must use supported storage.
* The source and target nodes must be available.
* The cluster should have quorum.
* Network connectivity between the nodes must be working.
* The administrator must have sufficient permissions.
* Target storage must have sufficient free capacity.

> **Warning:** A shorter replication interval increases replication activity and can increase storage and network usage.

---

# Procedure

## Step 1: Select the Guest

1. Log in to VM2Cloud VE.
2. Locate the required VM or container in the navigation tree.
3. Select the guest.
4. Open the **Replication** section.
5. Review the existing replication jobs.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Select the Replication Job

1. Locate the replication job that requires a schedule change.
2. Verify the guest.
3. Verify the target node.
4. Verify the current schedule.
5. Select the required replication job.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Open the Replication Job Configuration

1. Click **Edit**.
2. The replication-job configuration dialog opens.
3. Review the existing configuration.
4. Locate the **Schedule** field.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Configure the Schedule

1. Select the **Schedule** field.
2. Enter the required schedule.
3. Verify the schedule syntax.
4. Confirm that the schedule matches the required synchronization frequency.
5. Review the other replication settings.
6. Click **OK** or the corresponding save button displayed by the installed VM2Cloud VE version.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Schedule Configuration

VM2Cloud VE uses the underlying replication scheduler's calendar-event format.

A schedule can specify:

* Days.
* Start times.
* Repetition intervals.
* Multiple days.
* Multiple time values.

The schedule format is based on a subset of systemd calendar events.

---

# Schedule Limits

The underlying replication framework supports:

| Setting                      | Supported Range |
| ---------------------------- | --------------- |
| Minimum replication interval | 1 minute        |
| Maximum replication interval | 1 week          |

These limits apply to the replication scheduling mechanism, not to the amount of time required for the synchronization itself.

A replication job can take longer than its configured interval if the amount of data or available resources causes synchronization to take longer.

---

# Schedule Format

The general schedule format is:

```text
[day(s)] [[start-time(s)][/repetition-time(s)]]
```

The format is based on systemd calendar events.

---

## Days

Days can be specified using abbreviated English names:

```text
sun
mon
tue
wed
thu
fri
sat
```

Multiple days can be separated with commas.

Example:

```text
mon,wed,fri
```

A range can also be specified:

```text
mon..fri
```

If the day is omitted, all days are considered.

---

# Time Format

Times use the **24-hour format**.

Examples:

```text
08:00
13:30
22:00
```

For example:

```text
mon..fri 22:00
```

means that the replication job is scheduled for 22:00 from Monday through Friday.

The underlying documentation provides the equivalent example using:

```text
mon,tue,wed,thu,fri 22
```

which can be abbreviated to:

```text
mon..fri 22
```

---

# Repetition

A repetition value can be specified after a slash.

For example:

```text
8:00/15
```

means:

* Start at 08:00.
* Repeat every 15 minutes.

The schedule therefore starts at:

```text
08:00
08:15
08:30
08:45
09:00
```

The underlying documentation uses this format to demonstrate repeated replication within a time period.

---

# Common Schedule Examples

## Every Hour

```text
hourly
```

Use an appropriate supported schedule expression in the VM2Cloud VE interface for hourly replication.

---

## Every 15 Minutes Starting at 08:00

```text
8:00/15
```

This schedules repeated runs beginning at 08:00.

---

## Every Weekday at 22:00

```text
mon..fri 22:00
```

This schedules replication on Monday through Friday at 22:00.

---

## Specific Days

```text
mon,wed,fri 22:00
```

This schedules replication on Monday, Wednesday, and Friday at 22:00.

---

# Choosing a Replication Interval

The correct schedule depends on the workload.

Consider:

* Amount of data changed by the guest.
* Required recovery-point objective.
* Available network bandwidth.
* Storage performance.
* Number of replication jobs.
* Production traffic.
* Target-node capacity.

---

## Short Interval

Example:

```text
Every 5–15 minutes
```

Suitable when:

* The workload changes frequently.
* A small synchronization gap is required.
* Network and storage resources can support frequent replication.

Potential impact:

* Higher network usage.
* Higher storage activity.
* More frequent snapshots and synchronization operations.

---

## Medium Interval

Example:

```text
Every 30–60 minutes
```

Suitable for workloads where:

* Changes are moderate.
* A larger synchronization gap is acceptable.
* Network resources are limited.

---

## Long Interval

Example:

```text
Every few hours
```

Suitable when:

* Guest data changes slowly.
* Network bandwidth is limited.
* Very frequent synchronization is unnecessary.

---

# Schedule and Recovery Point

The replication schedule directly affects how much data may be unsynchronized when a source node fails.

For example:

```text
Replication
    |
    +---- 10:00
    |
    +---- 10:15
    |
    +---- 10:30
```

If the source node fails at 10:35, the target may not contain changes made after the latest successful synchronization.

Therefore:

```text
Shorter interval
       ↓
Smaller synchronization gap
```

but:

```text
Shorter interval
       ↓
More replication activity
```

Replication does not provide zero data loss. The underlying documentation explicitly notes that data can be lost between the last successful synchronization and a node failure.

---

# Schedule and Network Usage

Replication transfers changed data between the source and target nodes.

A shorter schedule can increase network utilization.

Before configuring an aggressive schedule:

1. Check network capacity.
2. Check replication traffic.
3. Check production traffic.
4. Check the number of replication jobs.
5. Check the bandwidth limits.
6. Monitor the network after applying the schedule.

---

# Schedule and Storage Usage

Replication uses snapshots to identify changes between synchronization runs.

The underlying replication mechanism uses snapshots and incremental synchronization after the initial synchronization.

A schedule should therefore be selected with storage activity in mind.

Monitor:

* Source storage utilization.
* Target storage utilization.
* Storage latency.
* Replication duration.
* Failed synchronization jobs.

---

# Schedule and Replication Duration

The configured schedule specifies when a replication job should start.

It does not guarantee that synchronization will finish before the next scheduled time.

For example:

```text
Schedule:
Every 15 minutes

Replication duration:
25 minutes
```

In this situation, the replication workload may overlap with subsequent scheduled times or be handled according to the replication scheduler's job execution behavior.

If replication consistently takes longer than the configured interval:

1. Review the replication status.
2. Check network bandwidth.
3. Check storage performance.
4. Check the amount of changed data.
5. Review the bandwidth limit.
6. Consider increasing the replication interval.

---

# Change the Replication Schedule

## Step 1: Open the Job

1. Select the required guest.
2. Open **Replication**.
3. Select the required replication job.
4. Click **Edit**.

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Step 2: Modify the Schedule

1. Locate **Schedule**.
2. Replace the existing schedule.
3. Enter the new schedule.
4. Verify the syntax.
5. Confirm that the schedule matches the intended replication frequency.

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

## Step 3: Save the Change

1. Review the configuration.
2. Click **OK** or the available save button.
3. Wait for the configuration dialog to close.
4. Confirm that the new schedule is displayed.

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

# Verification

After changing the schedule:

1. Open the guest.
2. Open **Replication**.
3. Select the replication job.
4. Verify the new schedule.
5. Confirm that the target node has not been changed unintentionally.
6. Confirm that the job remains enabled.
7. Wait for the next scheduled synchronization.
8. Open **Task History**.
9. Verify that the replication task starts according to the new schedule.
10. Verify that the synchronization completes successfully.

### Screenshot 8

```text
[ Place Screenshot Here ]
```

---

# Common Issues

## Invalid Schedule

Possible causes:

* Incorrect schedule syntax.
* Invalid day name.
* Invalid time.
* Unsupported repetition format.

Resolution:

1. Reopen the replication job.
2. Review the Schedule field.
3. Correct the syntax.
4. Save the configuration.
5. Verify the schedule again.

---

## Replication Does Not Run at the Expected Time

Possible causes:

* Incorrect schedule.
* Node time is incorrect.
* Time synchronization is not working.
* The replication job is disabled.
* A previous replication task is still running.
* Cluster or node problems exist.

Resolution:

1. Verify the replication schedule.
2. Check node time.
3. Check NTP/time synchronization.
4. Verify the replication job is enabled.
5. Check Task History.
6. Check replication status.
7. Check cluster status.

---

## Replication Consumes Too Much Network Bandwidth

Possible causes:

* Schedule is too frequent.
* Large amount of data changes between runs.
* Multiple replication jobs run simultaneously.
* No bandwidth limit is configured.

Resolution:

1. Increase the replication interval.
2. Configure an appropriate bandwidth limit.
3. Stagger replication schedules.
4. Monitor network utilization.
5. Verify replication still completes successfully.

---

## Replication Consumes Too Many Storage Resources

Possible causes:

* Frequent synchronization.
* Large amount of changed data.
* Slow replication completion.
* Insufficient target storage.

Resolution:

1. Review the replication schedule.
2. Check storage utilization.
3. Check replication duration.
4. Review the workload's write activity.
5. Increase the interval if appropriate.
6. Ensure sufficient storage capacity.

---

## Replication Runs During Peak Production Hours

If replication affects production performance:

1. Identify peak production periods.
2. Edit the replication schedule.
3. Move synchronization to an appropriate period.
4. Save the schedule.
5. Monitor production performance.
6. Verify successful replication.

---

# CLI Verification

CLI is secondary and should normally be used for troubleshooting or verification.

To view replication status:

```bash
pvesr status
```

To check scheduler status:

```bash
pvescheduler status
```

The underlying scheduler is responsible for starting scheduled jobs such as replication and backup jobs.

Use CLI verification only when the VM2Cloud VE UI does not provide enough information to diagnose the scheduling problem.

---

# Best Practices

* Select the replication interval according to the workload.
* Consider the required recovery-point objective.
* Monitor replication duration.
* Monitor network utilization.
* Monitor storage utilization.
* Avoid unnecessarily short intervals.
* Stagger multiple replication jobs when possible.
* Use bandwidth limits when required.
* Avoid scheduling heavy replication during critical production periods.
* Verify node time synchronization.
* Monitor failed replication tasks.
* Maintain independent backups.
* Test recovery procedures.

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

Time synchronization:

- [Time and Network Time (NTP)](../../03-Nodes/System/Time-and-NTP.md)

Cluster and HA:

- [Quorum](../Cluster/Quorum.md)
- [HA Overview](../HA/HA-Overview.md)

---

# Summary

Replication scheduling controls when VM2Cloud VE replication jobs synchronize guest data.

The underlying replication scheduler supports configurable intervals from **one minute to one week** and uses a subset of the systemd calendar-event format.

The general workflow is:

```text
Select Guest
    ↓
Open Replication
    ↓
Select Replication Job
    ↓
Click Edit
    ↓
Configure Schedule
    ↓
Save Changes
    ↓
Verify Schedule
    ↓
Monitor Next Synchronization
```

Choose the schedule based on:

* Recovery requirements.
* Guest write activity.
* Network capacity.
* Storage performance.
* Production workload.

A shorter interval can reduce the amount of unsynchronized data after a failure, but it also increases replication activity.
