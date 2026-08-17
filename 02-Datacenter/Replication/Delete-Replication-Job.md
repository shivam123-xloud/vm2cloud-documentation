# Delete Replication Job

---

## Overview

VM2Cloud VE allows administrators to delete an existing replication job for a virtual machine or container.

Deleting a replication job removes the replication configuration and prevents future scheduled synchronization for that job.

Deleting the replication job does **not** mean that the guest itself is deleted.

It is important to distinguish between:

- **Deleting the replication job** — removes the replication configuration.
- **Deleting the guest** — removes the VM or container.
- **Deleting replicated data** — removes data associated with the replica and is a separate destructive operation.

---

## When to Use

Delete a replication job when:

- The guest no longer needs replication.
- The target node is no longer required.
- The workload has been moved permanently.
- A replication configuration is no longer valid.
- A replication job was created by mistake.
- Replication needs to be redesigned.
- A guest is being decommissioned and its replication configuration is no longer required.

---

## Prerequisites

Before deleting a replication job:

- You must be logged in to VM2Cloud VE.
- You must have sufficient permissions to manage the replication configuration.
- The replication job must already exist.
- Verify that you are deleting the correct job.
- Verify the guest associated with the job.
- Verify the target node.
- Determine whether the existing replicated data is still required.

> **Warning:** Deleting a replication job stops future synchronization. If the target contains the only current copy of important data, verify your recovery and backup requirements before deleting the job.

---

# Procedure

## Step 1: Select the Guest

1. Log in to VM2Cloud VE.
2. Locate the VM or container associated with the replication job.
3. Select the guest from the navigation tree.
4. Open the **Replication** section.
5. Review the existing replication jobs.

### Screenshot 1

**Guest Replication View**

```text
[ Place Screenshot Here ]
```

> **Capture:** The guest's **Replication** tab with the job to be removed listed.

---

## Step 2: Select the Replication Job

1. Select the replication job to delete.
2. Confirm the job ID, guest, and target node.
3. Confirm this is the correct job.

### Screenshot 2

**Job Selected**

```text
[ Place Screenshot Here ]
```

> **Capture:** The job selected, its ID and target node visible so the right one is
> confirmed.

---

## Step 3: Remove the Job

1. Click **Remove**.
2. Review the confirmation message.
3. Confirm the deletion.

### Screenshot 3

**Removal Confirmation**

```text
[ Place Screenshot Here ]
```

> **Capture:** The confirmation dialog.

---

## Step 4: Verify the Deletion

1. Confirm the job no longer appears in the replication list.
2. Confirm no further replication runs are scheduled for that target.
3. Check the task log to confirm the removal completed.

### Screenshot 4

**Job Removed**

```text
[ Place Screenshot Here ]
```

> **Capture:** The replication list after deletion, showing the job gone.

---

# Replicated Data After Deletion

Removing a replication job stops future synchronization to the target node.

The data already replicated to the target is normally cleaned up as part of the removal. Depending on the VM2Cloud VE version and the state of the target node, residual replication snapshots or datasets may remain.

If the target node was offline when the job was deleted:

1. Bring the target node back online.
2. Verify whether replication data for the guest remains on the target storage.
3. Remove any leftover data manually if it is no longer required.

---

# Verification

Verify the following:

- The replication job no longer appears in the replication list.
- No new replication tasks are created for the deleted job.
- The guest continues to run normally on the source node.
- The removal task completed successfully.
- Target storage capacity is released once cleanup completes.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Job cannot be deleted | Verify you have permission to manage the guest's replication configuration. |
| Deletion fails while a run is active | Wait for the current replication run to finish, then delete the job. |
| Replication data remains on the target | Bring the target node online and remove leftover replication snapshots or datasets manually. |
| Job reappears in the list | Refresh the interface and confirm the removal task completed without error. |
| Guest can no longer be migrated quickly | Without replication, migration must transfer the full disk; recreate the job if fast migration is required. |

---

# Summary

Deleting a replication job stops all future synchronization of a guest to the configured target node and normally removes the replicated data from that target. Before deleting, confirm the target does not hold the only current copy of important data and that your backup strategy still meets recovery requirements. Verify after deletion that the job is gone and that the guest continues running normally on its source node.
