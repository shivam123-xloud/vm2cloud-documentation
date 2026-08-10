# Edit Replication Job

---

## Overview

VM2Cloud allows administrators to modify an existing replication job for a virtual machine or container.

Editing a replication job is used to change the replication configuration without creating a new job.

Common settings that may be changed include:

- Target node
- Replication schedule
- Bandwidth limit
- Job state or related replication options available in the installed VM2Cloud version

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

- You must be logged in to VM2Cloud.
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

1. Log in to VM2Cloud.
2. Locate the VM or container whose replication job must be changed.
3. Select the guest from the navigation tree.
4. Open the **Replication** section.
5. Review the existing replication jobs.

### Screenshot 1

```text
[ Place Screenshot Here ]
