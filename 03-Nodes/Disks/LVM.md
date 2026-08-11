# LVM

---

## Overview

**LVM (Logical Volume Manager)** is a block-level storage technology that allows VM2Cloud to use a physical disk or other block device through an LVM volume group.

LVM separates physical storage from logical volumes, allowing storage capacity to be managed through a volume group rather than assigning the entire physical disk directly to a single VM or container.

In VM2Cloud, an **LVM storage** definition uses an existing LVM volume group. The storage backend can provide VM disk images and container root directories. LVM storage uses raw volumes and does not provide VM2Cloud storage-level snapshots or clones.

LVM can also be used on top of shared block storage such as an iSCSI LUN, although the underlying storage design and cluster requirements must be considered carefully.

---

## When to Use

Use LVM when you need to:

* Create block-level storage for VMs.
* Create block-level storage for containers.
* Use an existing LVM volume group as VM2Cloud storage.
* Use raw logical volumes for guest disks.
* Use LVM on supported local block devices.
* Use LVM on top of a shared block device such as an iSCSI LUN when the environment requires it.

LVM is particularly useful when you need straightforward block storage without the snapshot and clone functionality provided by LVM-Thin.

---

## Prerequisites

Before configuring LVM storage:

* Log in to VM2Cloud with sufficient permissions.
* Ensure the required node is online.
* Ensure the target physical disk or block device is detected.
* An **LVM volume group must already exist**.
* The volume group must not already be incorrectly assigned to another storage configuration.
* Ensure sufficient free space exists in the volume group.
* If using shared storage, verify that the storage architecture supports the intended cluster configuration.
* Ensure required data is backed up before modifying existing LVM configuration.

> **Warning:** Creating, modifying, or removing LVM configuration can affect existing logical volumes and data. Never reuse a volume group until you have verified that it does not contain required data.

---

# Procedure

## Step 1: Select the Node

1. Log in to the VM2Cloud web interface.
2. Locate the resource tree.
3. Select the node where the LVM volume group is available.
4. Wait for the node management interface to load.

### Screenshot 1

```text
[ Place Screenshot Here — Select Node ]
```

---

## Step 2: Open Disks

1. In the node navigation menu, select **Disks**.
2. Open the disk-management page.
3. Review the available disk and storage-management options.

### Screenshot 2

```text
[ Place Screenshot Here — Node → Disks ]
```

---

## Step 3: Verify the LVM Volume Group

1. Open the LVM-related disk-management view.
2. Review the available volume groups.
3. Identify the volume group that will be used by VM2Cloud.
4. Verify the volume group name.
5. Verify that the volume group has sufficient free space.
6. Confirm that the volume group does not contain data that must be preserved before it is assigned to the intended storage configuration.

VM2Cloud's underlying storage manager can scan available LVM volume groups.

### Screenshot 3

```text
[ Place Screenshot Here — LVM Volume Groups ]
```

---

## Step 4: Add the LVM Storage

1. Select the appropriate **Datacenter** in the resource tree.
2. Open **Storage**.
3. Click **Add**.
4. Select **LVM**.
5. Enter the required storage configuration.
6. Select the existing LVM volume group.
7. Select the content types that the storage should provide.
8. Review the configuration.
9. Click **Add** to create the storage definition.

### Screenshot 4

```text
[ Place Screenshot Here — Datacenter → Storage → Add → LVM ]
```

---

# Configuration / Options

## ID

The **ID** is the unique name used by VM2Cloud to identify the storage.

Choose a meaningful identifier.

Example:

```text
local-lvm-data
```

The storage ID is used throughout the VM2Cloud interface when selecting storage for VM disks, container disks, and other supported content.

---

## Volume Group

The **Volume Group** identifies the existing LVM volume group that VM2Cloud will use.

The volume group must already exist.

For example:

```text
pve
```

The selected volume group becomes the backing storage for the VM2Cloud LVM storage definition.

The underlying LVM storage backend requires the `vgname` property to point to an existing volume group.

---

## Content

The **Content** setting determines what types of data the storage can contain.

For LVM storage, supported content includes:

* **Disk image**
* **Container**

The underlying storage model identifies VM images with `images` and container data with `rootdir`.

Select only the content types that are required.

---

## Nodes

Where available, the storage configuration can be restricted to specific cluster nodes.

This is important for local LVM because the underlying volume group exists on a particular node unless the storage is deliberately built on supported shared block storage.

Do not make a local LVM storage appear available on nodes that cannot actually access the underlying volume group.

---

## Disable

The storage can be disabled when it should temporarily not be used.

A disabled storage remains configured but is unavailable for normal storage operations until it is enabled again.

---

# Understanding LVM Storage

The basic LVM structure is:

```text
Physical Disk / Block Device
          │
          ▼
   Physical Volume (PV)
          │
          ▼
    Volume Group (VG)
          │
          ├── Logical Volume
          ├── Logical Volume
          └── Logical Volume
```

VM2Cloud uses the **volume group** as the storage backend.

Guest disks are allocated as logical volumes inside the volume group.

---

## Physical Volume

A **Physical Volume (PV)** is a disk or partition initialized for use by LVM.

Example:

```text
/dev/sdb
```

A physical volume can be added to an LVM volume group.

---

## Volume Group

A **Volume Group (VG)** combines one or more physical volumes into a pool of storage capacity.

Example:

```text
pve
```

VM2Cloud's LVM storage definition points to this volume group.

---

## Logical Volume

A **Logical Volume (LV)** is a virtual block device created inside a volume group.

VM2Cloud can create logical volumes for guest storage.

For example:

```text
Volume Group
    │
    ├── VM disk
    ├── VM disk
    └── Container root volume
```

---

# LVM Storage Characteristics

LVM is a **block-level storage** backend.

The underlying VM2Cloud/Proxmox storage documentation specifies the following characteristics for LVM:

| Feature                    | LVM                                                       |
| -------------------------- | --------------------------------------------------------- |
| Storage level              | Block                                                     |
| VM images                  | Supported                                                 |
| Container root directories | Supported                                                 |
| Image format               | Raw                                                       |
| Snapshots                  | Not supported by the storage backend                      |
| Clones                     | Not supported                                             |
| Shared storage             | Possible with appropriate underlying shared block storage |

---

# Snapshot and Clone Limitations

Standard LVM storage does not provide the same storage-level snapshot and clone support as LVM-Thin.

The underlying documentation specifically notes that the LVM backend does not support snapshots and clones, while LVM-Thin supports both.

If snapshot and clone functionality is required, consider **LVM-Thin** or another storage technology that supports those operations.

---

# Shared LVM

LVM can also be used on top of a shared block device such as an iSCSI LUN.

A common architecture is:

```text
Shared Storage
      │
      ▼
   iSCSI LUN
      │
      ▼
      LVM
      │
      ▼
 VM2Cloud Storage
```

This requires careful cluster-wide storage design and appropriate access from the participating nodes.

The underlying VM2Cloud/Proxmox storage documentation describes LVM on iSCSI as a way to obtain managed storage allocation on a shared LUN.

---

# Verification

After adding the LVM storage:

1. Select **Datacenter**.
2. Open **Storage**.
3. Locate the newly created LVM storage.
4. Confirm that the storage is enabled.
5. Confirm that the expected nodes can access it.
6. Select the storage and review its available content.
7. Confirm that the expected content types are enabled.
8. Verify that sufficient capacity is available.

For a functional test:

1. Create or select a test VM.
2. Open the VM's **Hardware** configuration.
3. Add a test disk.
4. Select the newly configured LVM storage.
5. Complete the disk-creation operation.
6. Verify that the disk appears in the VM hardware list.
7. Remove the test disk only after confirming that the storage works as expected.

### Screenshot 5

```text
[ Place Screenshot Here — LVM Storage After Creation ]
```

---

# Common Issues

## Volume Group Is Not Available

Possible causes include:

* The volume group does not exist.
* The disk containing the volume group is not detected.
* The volume group is inactive.
* The node cannot access the underlying block device.
* The volume group is already being used by another configuration.

### Resolution

1. Verify the physical disk is detected.
2. Verify the LVM volume group exists.
3. Check that the correct node is selected.
4. Verify that the volume group is accessible.
5. Reopen the LVM configuration page.
6. Retry the storage configuration.

---

## Storage Has No Free Space

If the LVM volume group is full:

* New VM disks cannot be allocated.
* New container storage cannot be allocated.
* Existing workloads may fail when they require additional space.

Monitor the available capacity regularly.

Do not overcommit storage without a clear capacity-management plan.

---

## Storage Is Visible on the Wrong Node

Local LVM storage depends on the physical availability of its underlying volume group.

If a volume group exists only on one node, the storage must not be treated as physically available on another node.

Check the storage's node restrictions and verify the underlying hardware on each node.

---

## VM Disk Cannot Be Created

Check:

1. The storage is enabled.
2. The storage is available on the selected node.
3. The selected content type includes VM images.
4. The volume group has sufficient free space.
5. The volume group is accessible.
6. The VM has sufficient permissions to use the storage.

---

## Container Root Disk Cannot Be Created

Check:

1. The storage is enabled.
2. The storage is available on the selected node.
3. **Container** content is enabled.
4. The volume group has sufficient free space.
5. The container operation has the required permissions.

---

## Existing LVM Data Must Be Preserved

Do not initialize, wipe, or recreate the volume group.

First identify:

* Existing physical volumes.
* Existing logical volumes.
* Existing storage definitions.
* VMs or containers using those volumes.

If existing data is required, stop the operation and follow the appropriate migration or storage-management procedure.

---

# Best Practices

* Use meaningful storage IDs.
* Verify the volume group before adding it to VM2Cloud.
* Do not reuse an existing volume group without checking its contents.
* Monitor free space regularly.
* Keep backups of important VM and container data.
* Do not expect standard LVM storage to provide storage-level snapshots.
* Use LVM-Thin when efficient snapshots and clones are required.
* Restrict local storage to nodes that can actually access the underlying volume group.
* For shared LVM, verify the complete shared-storage architecture before production use.
* Test new storage with a non-critical VM before deploying production workloads.

---

# Related Documentation

* [Disks Overview](Disks-Overview.md)
* [View Disk Information](View-Disk-Information.md)
* [Disk Management](Disk-Management.md)
* [LVM-Thin](LVM-Thin.md)
* [ZFS](ZFS.md)
* [Directory](Directory.md)
* [Disk Troubleshooting](Disk-Troubleshooting.md)
* [Storage Overview](../../02-Datacenter/Storage/Storage-Overview.md)
* [Add Storage](../../02-Datacenter/Storage/Add-Storage.md)

---

# Summary

LVM provides VM2Cloud with block-level storage based on an existing LVM volume group.

The storage configuration points VM2Cloud to the volume group, after which VM and container storage can be allocated as logical volumes.

Standard LVM storage does not provide storage-level snapshots or clones. When those capabilities are required, LVM-Thin or another supported storage technology should be considered.

Always verify the volume group, available capacity, node accessibility, and existing data before configuring LVM storage.
