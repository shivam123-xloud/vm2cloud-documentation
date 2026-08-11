# Delete Replication Job

---

## Overview

VM2Cloud allows administrators to delete an existing replication job for a virtual machine or container.

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

- You must be logged in to VM2Cloud.
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

1. Log in to VM2Cloud.
2. Locate the VM or container associated with the replication job.
3. Select the guest from the navigation tree.
4. Open the **Replication** section.
5. Review the existing replication jobs.

### Screenshot 1

```text
[ Place Screenshot Here ]
