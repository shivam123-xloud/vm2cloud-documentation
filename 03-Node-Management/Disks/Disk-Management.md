# Disk Management

---

## Overview

The **Disk Management** section in VM2Cloud is used to inspect and prepare physical disks attached to a node.

Administrators can use the available disk-management actions to identify disks, inspect their existing configuration, and prepare unused disks for supported storage technologies.

Disk-management operations can affect the contents of a physical disk. Operations such as wiping a disk are destructive and can permanently remove existing data and storage metadata.

VM2Cloud uses the underlying Proxmox VE disk-management functionality for these operations.

---

## When to Use

Use **Disk Management** when you need to:

* Inspect physical disks attached to a node.
* Determine whether a disk is available for use.
* Check existing disk configuration.
* Remove existing disk metadata before reuse.
* Prepare a disk for a new storage configuration.
* Troubleshoot disk-related storage problems.
* Verify disk state before creating LVM, LVM-Thin, ZFS, or directory storage.

---

## Prerequisites

Before managing a disk:

* Log in to VM2Cloud with an account that has the required node permissions.
* Ensure the target node is online.
* Confirm that the physical disk is connected and detected.
* Identify the correct physical disk.
* Verify whether the disk contains existing data.
* Ensure that required data has been backed up before destructive operations.

> **Warning:** Disk-management operations can permanently destroy data. Never wipe or reinitialize a disk until you have confirmed that the disk does not contain required data.

---

# Procedure

## Step 1: Select the Node

1. Log in to the VM2Cloud web interface.
2. Locate the resource tree.
3. Select the node containing the physical disk.
4. Wait for the node management interface to load.

### Screenshot 1

```text
[ Place Screenshot Here — Select Node ]
```

---

## Step 2: Open Disks

1. In the node navigation menu, select **Disks**.
2. Open the disk-management section.
3. Review the physical disks detected by the node.

### Screenshot 2

```text
[ Place Screenshot Here — Node → Disks ]
```

---

## Step 3: Identify the Target Disk

1. Locate the disk you want to manage.
2. Check its device identifier.
3. Check its capacity.
4. Check its model or other identifying information where displayed.
5. Determine whether the disk is already being used.
6. Confirm that the selected disk is the intended physical device.

Do not continue until the disk identity has been confirmed.

### Screenshot 3

```text
[ Place Screenshot Here — Identify Target Disk ]
```

---

# Disk Management Operations

The available actions depend on the VM2Cloud version, hardware, and current disk configuration.

---

## View Disk Information

Use the disk information view to inspect the selected physical disk.

Review information such as:

* Device name.
* Capacity.
* Model.
* Serial number, where available.
* Usage state.
* Partition information.
* Storage configuration.
* Health information, where supported.

Use this information to identify the disk before performing further operations.

---

## Check Disk Health

Where supported, inspect available disk health information.

Disk health information can help identify:

* Failing disks.
* SMART errors.
* Temperature problems.
* Other hardware-related conditions.

If a disk reports health problems, investigate the hardware before using it for production storage.

---

## Wipe a Disk

A disk-wiping operation removes existing partitioning and storage metadata from the selected disk.

### Before Wiping

1. Confirm the physical disk identity.
2. Confirm that the disk is not required by the operating system.
3. Confirm that no VM or container depends on the disk.
4. Confirm that required data has been backed up.
5. Confirm that the disk can safely be erased.

> **Warning:** Wiping a disk is destructive. Existing data and storage metadata may become inaccessible or be permanently destroyed.

### Wipe Procedure

1. Select the target node.
2. Open **Disks**.
3. Select the target disk.
4. Select the available **Wipe** action.
5. Review the warning or confirmation dialog.
6. Confirm that the displayed disk is the correct disk.
7. Confirm the destructive operation.
8. Wait for the task to complete.
9. Review the task result.

### Screenshot 4

```text
[ Place Screenshot Here — Disk Wipe Action ]
```

### Screenshot 5

```text
[ Place Screenshot Here — Wipe Confirmation ]
```

---

# Preparing a Disk for Storage

After identifying an unused physical disk, it can be used as the foundation for a supported storage configuration.

The appropriate method depends on the required storage technology.

Common choices include:

| Storage Technology | Typical Purpose                             |
| ------------------ | ------------------------------------------- |
| LVM                | Logical volume management                   |
| LVM-Thin           | Thin-provisioned VM/container storage       |
| ZFS                | Pooled storage with data-integrity features |
| Directory          | Filesystem-based storage                    |

Do not choose a storage technology solely because the disk is available. Select it according to workload requirements, redundancy requirements, capacity planning, and backup strategy.

---

# Configuration / Options

## Device

The device identifies the physical disk being managed.

Always verify this value before performing a destructive operation.

---

## Capacity

Capacity indicates the usable physical size reported by the node.

Use capacity together with device name and hardware information when identifying disks.

---

## Existing Configuration

Before modifying a disk, determine whether it contains:

* Partition tables.
* Filesystems.
* LVM metadata.
* LVM-Thin metadata.
* ZFS metadata.
* Existing storage data.

Existing configuration may prevent the disk from being used for a new storage configuration.

---

## Storage Type

When preparing a disk for storage, the selected storage type determines how the physical capacity will be managed.

For example:

* LVM uses physical volumes and volume groups.
* LVM-Thin uses a thin pool.
* ZFS uses storage pools.
* Directory storage uses a filesystem mounted on the node.

See the corresponding storage documentation for the complete configuration procedure.

---

# Verification

After completing a disk-management operation:

1. Confirm that the VM2Cloud task completed successfully.
2. Reopen or refresh the **Disks** page.
3. Verify that the disk shows the expected state.
4. Confirm that the intended storage configuration is visible if one was created.
5. Verify that no unexpected disk was modified.
6. For destructive operations, verify that the old storage metadata is no longer present before reusing the disk.

For storage creation, also verify the storage from the appropriate **Datacenter → Storage** interface.

---

# Common Issues

## Wipe Operation Fails

Possible causes include:

* Disk is currently in use.
* Disk is mounted.
* Active storage configuration exists.
* VM or container is using the storage.
* Hardware RAID controller prevents direct access.
* Insufficient permissions.
* Hardware or I/O failure.

### Resolution

1. Identify what is using the disk.
2. Stop dependent workloads if appropriate.
3. Remove the dependency using the correct storage procedure.
4. Verify that the disk can safely be modified.
5. Retry the operation.

---

## Disk Appears as Used

A disk may contain existing metadata even if it does not contain an obvious filesystem.

Possible metadata includes:

* LVM signatures.
* ZFS labels.
* Partition tables.
* Filesystem signatures.

Investigate the disk before wiping it.

---

## Disk Is Missing

If the disk is not listed:

1. Check the physical connection.
2. Check BIOS/UEFI detection.
3. Check the storage controller.
4. Check operating-system hardware detection.
5. Check for hardware failure.
6. Refresh the VM2Cloud interface.

---

## Storage Cannot Be Created on the Disk

Possible causes include:

* Disk already contains a storage configuration.
* Insufficient free space.
* Unsupported disk configuration.
* Disk is part of another storage.
* Hardware RAID configuration.
* Insufficient permissions.

Verify the disk state before attempting to create storage.

---

# Best Practices

* Always verify the disk identifier before modifying a disk.
* Never wipe a disk without confirming that its data is no longer required.
* Keep current backups before destructive operations.
* Do not use the system disk for destructive testing.
* Document physical disk assignments.
* Use appropriate storage technology for the workload.
* Verify storage after creating it.
* Monitor disk health regularly.
* Do not ignore SMART or hardware-health warnings.
* For production environments, design storage with appropriate redundancy and backup protection.

---

# Related Documentation

* [Disks Overview](01-Disks-Overview.md)
* [View Disk Information](02-View-Disk-Information.md)
* [LVM](04-LVM.md)
* [LVM-Thin](05-LVM-Thin.md)
* [ZFS](06-ZFS.md)
* [Directory](07-Directory.md)
* [Disk Troubleshooting](08-Disk-Troubleshooting.md)
* [Storage Overview](../06-Storage/Storage-Overview.md)

---

# Summary

The **Disk Management** section is used to inspect and prepare physical disks on a VM2Cloud node.

Administrators should first identify the target disk and determine whether it is already being used. Destructive operations such as wiping must only be performed after confirming that the disk contains no required data.

After completing a disk-management operation, always verify the resulting disk state and confirm that the intended storage configuration is functioning correctly.
