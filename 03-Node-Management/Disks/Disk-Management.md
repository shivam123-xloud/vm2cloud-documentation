# Disk Management

---

## Overview

The **Disk Management** section allows administrators to perform management operations on physical disks detected by a VM2Cloud node.

Disk management operations are used when preparing physical disks for VM2Cloud storage configurations or when removing existing disk configuration.

Some operations are destructive and can permanently remove data. Always verify the target disk before performing any operation.

---

## When to Use

Use **Disk Management** to:

- Prepare an unused physical disk.
- Initialize a disk.
- Remove existing disk information when required.
- Wipe a disk before reusing it.
- Prepare a disk for LVM, LVM-Thin, ZFS, or another storage configuration.
- Verify the result of a disk operation.

---

## Prerequisites

Before managing a disk:

- Log in to the VM2Cloud web interface.
- Select the required node.
- Ensure the node is online.
- Have administrative permissions.
- Identify the correct physical disk.
- Confirm that the disk does not contain required data.
- Ensure backups exist before performing destructive operations.

---

# View Available Disks

## Step 1: Open the Node

1. Log in to the VM2Cloud web interface.
2. Select the required node from the left navigation panel.
3. Expand the node.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
