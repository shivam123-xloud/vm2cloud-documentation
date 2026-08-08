# Disks Overview

---

## Overview

The **Disks** section allows administrators to view and manage physical disks available on a VM2Cloud node.

It provides information about the node's storage devices and provides access to disk-management operations used when preparing disks for storage configurations such as LVM, LVM-Thin, ZFS, and Directory storage.

Disk operations should be performed carefully because modifying or initializing a disk can destroy existing data.

---

## When to Use

Use the **Disks** section to:

- View physical disks installed in the node.
- Check disk capacity and status.
- Identify available disks.
- Review disk partitions.
- Prepare unused disks for storage configuration.
- Troubleshoot disk-related problems.

---

## Prerequisites

Before managing disks, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have administrative privileges.
- The selected node is online.
- You know which physical disk you want to manage.
- Important data has been backed up before destructive operations.

---

# Open the Disks Page

## Step 1: Select the Node

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Expand **Disks**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Available Disk Management Options

The Disks section may provide separate views for different storage technologies.

Common sections include:

- Disks
- LVM
- LVM-Thin
- Directory
- ZFS

The available options depend on the installed storage components and the node configuration.

---

# View Physical Disks

Open **Disks** to view the physical storage devices detected by the node.

Typical information includes:

- Device name
- Disk size
- Model
- Serial number
- Usage
- Health or status
- Partition information

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Identify an Unused Disk

Before using a disk for VM2Cloud storage:

1. Open **Disks**.
2. Review the available devices.
3. Identify the disk that is not currently being used.
4. Verify its size and device information.
5. Confirm that it does not contain required data.

Do not initialize or format a disk until you have confirmed that it is safe to use.

---

# Disk Management Operations

Depending on the VM2Cloud version and disk state, available operations may include:

- View disk information.
- Initialize disk with GPT.
- Wipe disk.
- Create or remove partitions.
- View S.M.A.R.T. information.
- Prepare the disk for storage configuration.

---

## Warning

Disk operations can permanently remove data.

Always verify:

- Correct node.
- Correct physical disk.
- Correct device name.
- Existing data and partitions.
- Backup availability.

Do not perform destructive disk operations on a production disk unless the operation is planned and approved.

---

# Storage Technologies

Physical disks can be used as the foundation for different VM2Cloud storage configurations.

| Storage Type | Purpose |
|---|---|
| LVM | Provides logical volume management for VM2Cloud storage. |
| LVM-Thin | Provides thin-provisioned storage for virtual disks. |
| ZFS | Provides advanced storage features such as redundancy, snapshots, and data integrity. |
| Directory | Uses a filesystem directory to store VM2Cloud content. |

The appropriate storage type depends on the environment, hardware, performance requirements, and redundancy requirements.

---

# Verification

After performing a disk operation, verify:

- The expected disk is displayed.
- The disk size is correct.
- The disk state reflects the performed operation.
- The disk is available for the intended storage configuration.
- No unexpected disks or partitions were modified.

---

# Common Issues

| Issue | Resolution |
|---|---|
| Disk is not displayed | Verify that the disk is detected by the operating system and check the physical connection. |
| Disk shows existing partitions | Verify whether the data is required before performing any destructive operation. |
| Disk cannot be initialized | Check whether the disk is currently being used by another storage configuration. |
| Disk appears unavailable | Check whether it is already assigned to LVM, LVM-Thin, ZFS, or another storage configuration. |
| Disk operation fails | Review the task output and system logs for additional information. |

---

# Best Practices

- Never assume an unused-looking disk is safe to erase.
- Verify disk serial numbers before destructive operations.
- Keep backups of important data.
- Use appropriate redundancy for production storage.
- Monitor disk health regularly.
- Document physical disk assignments.
- Avoid modifying disks that are already part of active storage.

---

# Related Documentation

- LVM
- LVM-Thin
- ZFS
- Directory Storage
- Storage Management
- Disk Troubleshooting

---

# Summary

The **Disks** section provides administrators with visibility and management of the physical storage devices available on a VM2Cloud node. It is the starting point for preparing disks for storage technologies such as LVM, LVM-Thin, ZFS, and Directory storage.

Because disk operations can permanently destroy data, administrators must verify the target disk before performing any destructive operation.
