# Edit Replication Job

---

## Overview

VM2Cloud VE allows administrators to modify an existing replication job for a virtual machine or container.

Editing a replication job is used to change the replication configuration without creating a new job.

Common settings that may be changed include:

- Target node
- Replication schedule
- Bandwidth limit
- Job state (enabled or disabled)

Replication changes affect how future synchronization jobs are performed. Existing replicated data is not automatically replaced simply because a configuration value is edited.

---

## When to Use

Edit a replication job when:

- The replication schedule needs to be changed.
- The target node needs to be changed.
- A bandwidth limit needs to be adjusted.
- Replication behavior needs to be modified.
- The workload requirements have changed.
- Network or storage capacity has changed.
- A replication job needs to be adjusted after infrastructure changes.

---

## Prerequisites

Before editing a replication job:

- You must be logged in to VM2Cloud VE.
- You must have sufficient permissions to modify the guest and replication configuration.
- The guest must exist.
- The replication job must already exist.
- The source node must be accessible.
- The cluster should have quorum.
- The target node should be online when changing or validating the target.
- The target storage must have sufficient capacity.
- The source and target nodes must have network connectivity.

> **Warning:** Changing replication configuration can affect future synchronization. Verify the new configuration before saving it.

---

# Procedure

## Step 1: Select the Guest

1. Log in to VM2Cloud VE.
2. Locate the VM or container whose replication job must be changed.
3. Select the guest from the navigation tree.
4. Open the **Replication** section.
5. Review the existing replication jobs.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Select the Replication Job

1. Select the replication job to modify.
2. Confirm the job ID, target node, and schedule.
3. Click **Edit**.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Modify the Configuration

Change the required settings.

Editable settings typically include:

- **Schedule** — how often replication runs.
- **Rate limit** — the maximum bandwidth the job may use.
- **Comment** — an optional description.
- **Enabled** — whether the job is active.

> **Note:** The target node cannot be changed on an existing job in most VM2Cloud VE versions. To replicate to a different node, create a new job for that target and remove the old one.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Save the Changes

1. Review the modified configuration.
2. Click **OK**.
3. Wait for the configuration to be applied.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Step 5: Verify the Updated Job

1. Confirm the job list shows the new values.
2. Check the next scheduled run time.
3. Wait for the next run, or trigger one manually, and confirm it completes.

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The replication job shows the updated configuration.
- The next run time reflects the new schedule.
- The job remains enabled if it should be active.
- The next replication run completes successfully.
- The target node reports a current synchronization time.
- No replication errors appear in the task log.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Changes are not saved | Verify you have permission to modify the guest's replication configuration. |
| Job does not run at the new schedule | Confirm the schedule syntax is valid and that the node time is synchronized. |
| Replication fails after editing | Review the task log and verify the target storage still has capacity. |
| Bandwidth limit has no effect | Confirm the limit was applied to the correct job and that the value is in the expected unit. |
| Target node cannot be changed | Create a new job for the required target node and remove the previous job. |

---

# Summary

Editing a replication job lets administrators adjust the schedule, bandwidth limit, and state of an existing job without recreating it or resynchronizing the guest from scratch. Because the target node is fixed once a job exists, changing the destination requires creating a new job and removing the old one. Verify the next scheduled run after any change to confirm the job still completes successfully.
