# Directory

---

## Overview

**Directory storage** is a file-level storage backend that allows VM2Cloud VE to use a directory on a node as storage for virtual machines, containers, templates, ISO images, backups, and other supported content.

The directory can be located on:

* A local filesystem.
* A locally mounted filesystem.
* A filesystem mounted through standard Linux mount configuration.

Because directory storage operates at the file level, it can support a wide range of content types. However, the directory backend itself does not provide native storage-level snapshots.

---

## When to Use

Use Directory storage when you need to:

* Store VM disk images as files.
* Store container root filesystems.
* Store ISO images.
* Store container templates.
* Store VM templates.
* Store backup files.
* Use an existing Linux filesystem as VM2Cloud VE storage.
* Use a locally mounted filesystem as storage.
* Store files on filesystems supported by Linux.

Directory storage is useful when flexibility and support for multiple content types are more important than block-level storage features.

---

## Prerequisites

Before configuring Directory storage:

* Log in to VM2Cloud VE with sufficient permissions.
* Ensure the target node is online.
* Ensure the target directory exists.
* Ensure the filesystem is mounted and accessible.
* Ensure sufficient free disk space exists.
* Verify that the directory is not already being used by another incompatible storage configuration.
* Ensure the VM2Cloud VE system has permission to access the directory.
* Back up important data before modifying an existing filesystem or mount point.

If the directory is located on another filesystem or storage device, mount that filesystem on the node before creating the Directory storage definition.

> **Warning:** Do not configure a directory as VM2Cloud VE storage until you have verified that it points to the intended filesystem. Incorrectly selecting a system directory or an existing directory containing unrelated data can cause data loss or unexpected storage behavior.

---

# Procedure

## Step 1: Select the Node

1. Log in to the VM2Cloud VE web interface.
2. Locate the resource tree.
3. Select the node where the directory exists.
4. Wait for the node management interface to load.

### Screenshot 1

```text
[ Place Screenshot Here — Select Node ]
```

---

## Step 2: Verify the Directory

Before adding the directory as VM2Cloud VE storage, verify that the required directory exists on the selected node.

The directory must be accessible from the node and have sufficient capacity.

If the directory is located on a separate filesystem:

1. Verify that the filesystem is mounted.
2. Verify the mount point.
3. Verify available capacity.
4. Verify filesystem accessibility.
5. Confirm that the mount will remain available after a node reboot.

The underlying Directory backend can use local directories or locally mounted shares. Additional filesystems can be mounted using standard Linux `/etc/fstab` configuration and then defined as Directory storage.

---

## Step 3: Open Datacenter Storage

1. Select **Datacenter** in the resource tree.
2. Select **Storage**.
3. Click **Add**.
4. Select **Directory**.

### Screenshot 2

```text
[ Place Screenshot Here — Datacenter → Storage → Add → Directory ]
```

---

## Step 4: Configure Directory Storage

Enter the required storage configuration.

Configure:

1. Storage ID.
2. Directory path.
3. Content types.
4. Node availability.
5. Any additional supported options displayed by the current VM2Cloud VE version.

### Screenshot 3

```text
[ Place Screenshot Here — Directory Storage Configuration ]
```

---

## Step 5: Configure the Storage ID

1. Enter a unique storage ID.
2. Use a meaningful name that identifies the storage purpose.

Example:

```text
local-directory
```

The storage ID is used throughout VM2Cloud VE when selecting storage for VMs, containers, ISO images, templates, and backups.

---

## Step 6: Configure the Directory Path

1. Enter or select the directory path used for storage.
2. Verify that the path exists on the selected node.
3. Verify that the path points to the intended filesystem.
4. Do not select a system directory unless it is intentionally being used as VM2Cloud VE storage.

Example:

```text
/mnt/vm2cloud-storage
```

The underlying Directory backend requires a valid filesystem path.

### Screenshot 4

```text
[ Place Screenshot Here — Directory Path ]
```

---

## Step 7: Select Content Types

Select the content types that should be stored in the directory.

Directory storage supports a broad range of content types, including:

* **Disk images**
* **Container root directories**
* **ISO images**
* **Container templates**
* **Backup files**
* **VM templates**
* Other supported file-based content depending on the VM2Cloud VE version.

The underlying Directory backend supports virtual disk images, containers, templates, ISO images, and backup files.

Select only the content types required for the intended storage.

### Screenshot 5

```text
[ Place Screenshot Here — Directory Content Types ]
```

---

## Step 8: Configure Node Availability

If the directory is local to one node:

1. Select the node that can access the directory.
2. Verify that the directory exists on that node.
3. Do not make the storage available to nodes that cannot access the same directory.

If multiple nodes must access the same data, the underlying filesystem must actually be shared and mounted appropriately on every participating node.

A local directory is not automatically shared simply because the VM2Cloud VE nodes belong to the same cluster.

---

## Step 9: Add the Storage

1. Review the storage ID.
2. Verify the directory path.
3. Review the selected content types.
4. Verify node availability.
5. Confirm that the directory is the intended filesystem.
6. Click **Add**.
7. Wait for the storage task to complete.

### Screenshot 6

```text
[ Place Screenshot Here — Add Directory Storage ]
```

---

# Configuration / Options

## Storage ID

The **Storage ID** uniquely identifies the directory storage inside VM2Cloud VE.

Example:

```text
local-dir
```

Use a consistent naming convention.

---

## Directory

The **Directory** field specifies the filesystem path used by the storage.

Example:

```text
/mnt/storage
```

The path must exist and be accessible on the node.

---

## Content

The **Content** setting determines which types of data can be stored.

Directory storage can support multiple content types because it is file-level storage.

Common content types include:

| Content            | Purpose                              |
| ------------------ | ------------------------------------ |
| Disk image         | VM virtual disks                     |
| Container          | Container root filesystems           |
| ISO image          | Installation ISO files               |
| Container template | Container templates                  |
| Backup             | Backup archives                      |
| Snippets           | Configuration or cloud-init snippets |
| VZDump backup      | VM/container backup files            |

Only enable content types that are appropriate for the storage.

---

## Nodes

The **Nodes** setting controls which nodes can use the storage definition.

For local Directory storage, only nodes that can access the underlying path should be selected.

If the same shared filesystem is mounted at the same path on multiple nodes, the storage can be configured for those nodes after verifying that the filesystem is genuinely shared and accessible.

---

## Disable

Disabling storage prevents normal VM2Cloud VE storage operations from using the storage while retaining its configuration.

Disabling storage does not delete the underlying directory or its contents.

---

# Directory Storage Layout

VM2Cloud VE organizes different content types into directories below the configured storage path.

A simplified structure can look like:

```text
/mnt/vm2cloud-storage/
├── images/
├── dump/
├── template/
│   ├── iso/
│   └── cache/
├── snippets/
└── private/
```

The exact directory structure depends on the content stored and the VM2Cloud VE version.

Do not manually rename or move VM2Cloud VE-managed storage files unless the operation is explicitly supported.

---

# VM Disk Storage

Directory storage can store VM disk images as files.

For example:

```text
VM
 │
 ▼
Directory Storage
 │
 ▼
VM disk image file
```

The underlying Directory backend is file-based and supports virtual disk images.

The exact disk image format depends on the disk configuration and selected storage options.

---

# Container Storage

Directory storage can also store container root filesystems.

A container's root filesystem is stored within the configured directory storage.

This allows Directory storage to be used for both VM and container workloads.

---

# ISO Images

Directory storage can be used to store ISO installation images.

Typical workflow:

1. Open the storage.
2. Select the appropriate content view.
3. Upload or otherwise place the ISO on the storage.
4. Verify that the ISO appears in the storage content list.
5. Select the ISO when creating or configuring a VM.

---

# Container Templates

Directory storage can store container templates.

Typical workflow:

1. Open the storage.
2. Select the container-template content.
3. Upload or download the required template through the supported VM2Cloud VE workflow.
4. Verify that the template appears.
5. Use the template when creating a container.

---

# Backup Storage

Directory storage can store VM2Cloud VE backup files.

Typical workflow:

1. Select the required backup storage.
2. Create a VM or container backup.
3. Select the Directory storage as the target.
4. Start the backup.
5. Wait for the backup task to complete.
6. Verify the backup file in the storage content view.

---

# Storage-Level Snapshots

Directory storage does **not** provide native storage-level snapshots.

This is an important difference from storage technologies such as:

* LVM-Thin.
* ZFS.
* Other storage backends that implement native snapshots.

The underlying Directory backend assumes a POSIX-compatible filesystem and does not provide storage-level snapshots.

For VM images using the QCOW2 format, snapshots can be supported internally by the image format, but this is different from native storage-level snapshots.

---

# Filesystem Requirements

The underlying Directory backend assumes that the directory is POSIX-compatible.

This means VM2Cloud VE relies on the underlying Linux filesystem for:

* File creation.
* File permissions.
* Directory access.
* File allocation.
* Filesystem capacity.
* Filesystem availability.

The underlying platform can use filesystems supported by Linux when they are mounted and configured as Directory storage.

---

# Mounting External Filesystems

A separate filesystem can be mounted on the node and then used as Directory storage.

For example:

```text
External Filesystem
        │
        ▼
   Linux Mount
        │
        ▼
/mnt/vm2cloud-storage
        │
        ▼
 VM2Cloud VE Directory Storage
```

The underlying documentation specifically allows additional storage to be mounted through standard Linux `/etc/fstab` configuration and then defined as Directory storage.

When using an external filesystem:

* Ensure it mounts before VM2Cloud VE storage operations begin.
* Use a stable mount point.
* Verify filesystem availability after reboot.
* Ensure the storage path is not accidentally mounted to the wrong filesystem.
* Monitor filesystem capacity.

---

# Cache Mode Consideration

Some filesystems do not support `O_DIRECT`.

When a storage filesystem does not support `O_DIRECT`, the underlying documentation recommends using **writeback** instead of cache mode **none** for affected storage.

When configuring VM disks:

1. Check the filesystem capabilities.
2. Check the VM disk cache configuration.
3. Avoid using an unsupported cache mode.
4. Use the appropriate cache mode for the underlying filesystem.

---

# Verification

After adding Directory storage:

1. Select **Datacenter**.
2. Open **Storage**.
3. Select the newly created Directory storage.
4. Verify that the storage is enabled.
5. Verify the correct node availability.
6. Verify the configured directory path.
7. Verify the enabled content types.
8. Confirm that the storage reports available capacity.

### Screenshot 7

```text
[ Place Screenshot Here — Directory Storage Verification ]
```

---

## Functional Verification

### Test VM Disk

1. Create or select a test VM.
2. Open **Hardware**.
3. Select **Add → Hard Disk**.
4. Select the Directory storage.
5. Configure the required disk size and options.
6. Complete the operation.
7. Verify that the disk appears in the VM hardware list.
8. Start the VM if appropriate.
9. Verify that the guest can access the disk.

---

### Test ISO Storage

1. Select the Directory storage.
2. Open its content view.
3. Select the ISO-related content.
4. Upload a test ISO using the supported VM2Cloud VE workflow.
5. Wait for the upload task to complete.
6. Verify that the ISO appears in the storage content list.
7. Confirm that the ISO can be selected when configuring a VM.

---

### Test Backup Storage

1. Select a test VM.
2. Start a backup.
3. Select the Directory storage as the backup target.
4. Wait for the backup task to complete.
5. Open the storage content view.
6. Confirm that the backup appears.
7. Verify that the backup can be selected for a restore operation.

---

# Common Issues

## Directory Does Not Exist

Possible causes:

* Incorrect path.
* Filesystem is not mounted.
* Mount operation failed.
* Directory was removed.
* Typographical error in the path.

### Resolution

1. Verify the configured directory path.
2. Verify that the filesystem is mounted.
3. Confirm that the directory exists.
4. Correct the path if required.
5. Refresh the storage configuration.
6. Retry the operation.

---

## Storage Shows No Free Space

Possible causes:

* Filesystem is full.
* VM disks are consuming the available capacity.
* Backup files are consuming space.
* ISO files or templates are consuming space.
* Snapshots or image files are consuming additional space.

### Resolution

1. Review storage usage.
2. Identify large files or workloads.
3. Remove only data that is no longer required.
4. Move workloads to another storage if appropriate.
5. Extend the underlying filesystem when possible.
6. Recheck available capacity.

---

## Storage Is Offline

Possible causes:

* Filesystem is not mounted.
* Mount point changed.
* Storage device is unavailable.
* Network filesystem is unavailable.
* Permissions changed.
* Underlying filesystem has errors.

### Resolution

1. Verify node connectivity.
2. Verify the filesystem.
3. Verify the mount point.
4. Check storage hardware or remote storage availability.
5. Verify filesystem permissions.
6. Review VM2Cloud VE task history.
7. Restore filesystem availability.
8. Recheck the storage.

---

## VM Disk Cannot Be Created

Check:

1. Directory storage is enabled.
2. The selected node can access the directory.
3. **Disk image** content is enabled.
4. Sufficient free space exists.
5. The filesystem is writable.
6. The user has sufficient permissions.
7. The storage is not in an error state.

---

## ISO Upload Fails

Possible causes:

* Insufficient free space.
* Directory is read-only.
* Incorrect permissions.
* Network interruption.
* Storage became unavailable.

### Resolution

1. Check available storage capacity.
2. Verify that the filesystem is writable.
3. Verify node connectivity.
4. Check task history.
5. Retry the upload after correcting the underlying issue.

---

## Backup Fails

Check:

1. Storage is online.
2. Sufficient free space exists.
3. Backup content is enabled.
4. The directory is writable.
5. The VM or container is accessible.
6. The backup task has the required permissions.

---

## Storage Is Available on the Wrong Node

If the storage uses a local directory, another node cannot automatically access it.

### Resolution

1. Check the storage's node configuration.
2. Verify the physical location of the directory.
3. Remove nodes that cannot access the directory.
4. If shared access is required, configure an appropriate shared filesystem and mount it correctly on every participating node.

---

# CLI Verification

The VM2Cloud VE UI should be used for normal Directory storage management.

CLI commands may be useful when troubleshooting filesystem or mount problems.

## Check Filesystem Capacity

```bash
df -h /mnt/vm2cloud-storage
```

Replace the path with the configured Directory storage path.

---

## Check Mounts

```bash
findmnt /mnt/vm2cloud-storage
```

This verifies whether the expected filesystem is mounted at the configured path.

---

## Check Directory Permissions

```bash
ls -ld /mnt/vm2cloud-storage
```

Use this when investigating access or permission problems.

---

## Check VM2Cloud VE Storage Status

```bash
pvesm status
```

This can be used to verify whether the storage is detected and available. The underlying platform provides `pvesm status` for listing storage status.

---

## List Storage Contents

```bash
pvesm list <STORAGE_ID>
```

Replace `<STORAGE_ID>` with the VM2Cloud VE storage ID.

The underlying storage manager supports listing storage contents with `pvesm list`.

> **Note:** CLI commands in this document are for verification and troubleshooting. Use the VM2Cloud VE web interface for normal storage configuration whenever the UI provides the required operation.

---

# Best Practices

* Use a dedicated directory or filesystem for production storage.
* Avoid using arbitrary system directories as VM2Cloud VE storage.
* Verify the mount point before adding it as storage.
* Use stable filesystem mount configuration for external filesystems.
* Monitor filesystem capacity.
* Keep sufficient free space for VM operations.
* Keep backups independent from the primary VM storage when possible.
* Do not treat local Directory storage as shared storage.
* Restrict storage to nodes that can actually access the configured path.
* Verify filesystem permissions.
* Do not manually move VM2Cloud VE-managed files unless the operation is supported.
* Test the storage with a non-production workload before production deployment.
* Monitor filesystem health and underlying hardware.
* Use an appropriate cache mode for the underlying filesystem.

---

# Related Documentation

* [Disks Overview](Disks-Overview.md)
* [View Disk Information](View-Disk-Information.md)
* [Disk Management](Disk-Management.md)
* [LVM](LVM.md)
* [LVM-Thin](LVM-Thin.md)
* [ZFS](ZFS.md)
* [Disk Troubleshooting](Disk-Troubleshooting.md)
* [Storage Overview](../../02-Datacenter/Storage/Storage-Overview.md)
* [Add Storage](../../02-Datacenter/Storage/Add-Storage.md)
* [Storage Content](../../02-Datacenter/Storage/Upload-Content.md)

---

# Summary

Directory storage provides VM2Cloud VE with flexible file-level storage based on a directory on the node.

It can store multiple content types, including VM disks, container data, ISO images, templates, and backup files. The directory can be located on a local filesystem or on a filesystem mounted on the node.

Directory storage does not provide native storage-level snapshots. Its capabilities therefore differ from block- and pool-based storage technologies such as LVM-Thin and ZFS.

Before using Directory storage in production, verify the filesystem, mount point, permissions, capacity, node availability, and backup strategy.
