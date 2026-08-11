# LVM-Thin

---

## Overview

**LVM-Thin** is a block-level storage technology that provides thin-provisioned storage for VM2Cloud virtual machines and containers.

Unlike standard LVM, LVM-Thin allocates physical storage blocks as data is written instead of allocating the complete requested volume size immediately. This allows a virtual disk to have a configured size larger than the physical space currently consumed by its data.

LVM-Thin also provides efficient support for storage-level snapshots and clones.

The VM2Cloud LVM-Thin storage backend uses an existing LVM volume group and an LVM thin pool as its storage source.

---

## When to Use

Use LVM-Thin when you need:

* Thin-provisioned VM storage.
* Thin-provisioned container storage.
* Efficient VM snapshots.
* Efficient VM clones.
* Block-level storage.
* Local storage for a VM2Cloud node.
* Flexible allocation of storage capacity.
* Efficient use of physical storage for virtual disks.

LVM-Thin is particularly useful when snapshots and cloning are required and the storage does not need to be shared directly between multiple nodes.

---

## Prerequisites

Before configuring LVM-Thin:

* Log in to VM2Cloud with sufficient permissions.
* Ensure the target node is online.
* Ensure the required physical disk or block device is available.
* An LVM volume group must exist.
* An LVM thin pool must exist inside the volume group.
* Ensure sufficient free space exists in the thin pool.
* Ensure sufficient metadata capacity is available.
* Confirm that the volume group and thin pool are not being used by an existing storage configuration that must be preserved.
* Ensure required data is backed up before modifying existing LVM configuration.

> **Warning:** Creating, modifying, resizing, or removing an LVM-Thin pool can affect all virtual disks stored in that pool. Never remove or recreate an existing thin pool until all dependent workloads and data have been verified.

---

# Procedure

## Step 1: Select the Node

1. Log in to the VM2Cloud web interface.
2. Locate the resource tree.
3. Select the node where the LVM-Thin storage will be configured.
4. Wait for the node management interface to load.

### Screenshot 1

```text
[ Place Screenshot Here — Select Node ]
```

---

## Step 2: Open Disks

1. In the node navigation menu, select **Disks**.
2. Open the disk-management page.
3. Review the available disk-management options.

### Screenshot 2

```text
[ Place Screenshot Here — Node → Disks ]
```

---

## Step 3: Verify the LVM Volume Group

1. Open the LVM-related disk-management view.
2. Review the available volume groups.
3. Identify the volume group that will contain the thin pool.
4. Verify the volume group name.
5. Check the available capacity.
6. Confirm that the volume group is the correct one.

The LVM-Thin storage backend requires an existing volume group.

### Screenshot 3

```text
[ Place Screenshot Here — LVM Volume Group ]
```

---

## Step 4: Verify or Create the Thin Pool

An LVM-Thin storage definition requires a **thin pool** inside the selected volume group.

1. Open the LVM-Thin management area.
2. Review the available thin pools.
3. If an appropriate thin pool already exists, verify its name and capacity.
4. If a new thin pool is required, create it using the supported VM2Cloud disk-management workflow.
5. Assign the required capacity to the thin pool.
6. Confirm the operation.
7. Wait for the task to complete.
8. Verify that the new thin pool appears in the available LVM-Thin resources.

### Screenshot 4

```text
[ Place Screenshot Here — LVM-Thin Pool Configuration ]
```

The underlying platform documentation describes an LVM-Thin pool as an LVM logical volume configured as a thin pool.

---

## Step 5: Add the LVM-Thin Storage

1. Select **Datacenter** in the resource tree.
2. Open **Storage**.
3. Click **Add**.
4. Select **LVM-Thin**.
5. Enter the storage configuration.
6. Select the appropriate volume group.
7. Select the appropriate thin pool.
8. Select the content types required for the storage.
9. Review the configuration.
10. Click **Add**.
11. Wait for the task to complete.

### Screenshot 5

```text
[ Place Screenshot Here — Datacenter → Storage → Add → LVM-Thin ]
```

---

# Configuration / Options

## ID

The **ID** is the unique name used by VM2Cloud to identify the LVM-Thin storage.

Use a meaningful name.

Example:

```text
local-lvm
```

The storage ID is used when selecting storage for VM and container disks.

---

## Volume Group

The **Volume Group** identifies the existing LVM volume group containing the thin pool.

The selected volume group must exist before the LVM-Thin storage is configured.

Example:

```text
pve
```

The underlying LVM-Thin backend requires the volume-group name to reference an existing volume group.

---

## Thin Pool

The **Thin Pool** identifies the LVM thin pool that VM2Cloud will use for guest storage.

Example:

```text
data
```

The underlying storage configuration uses the `thinpool` property to identify the LVM thin pool.

---

## Content

The **Content** setting determines which types of guest storage can be created on the LVM-Thin storage.

The LVM-Thin backend supports:

* **Disk images**
* **Container root directories**

The underlying storage configuration identifies these content types as `images` and `rootdir`.

---

## Nodes

The storage can be restricted to specific nodes.

This is important because LVM-Thin storage is local to the node containing the underlying volume group and thin pool.

Do not make an LVM-Thin storage available to a node that cannot access the underlying thin pool.

The underlying platform documentation states that LVM-Thin pools cannot be shared across multiple nodes.

---

## Disable

The storage can be disabled when it should temporarily not be used.

A disabled storage remains configured but cannot be used for normal storage operations until it is enabled again.

---

# Understanding LVM-Thin

The storage hierarchy is:

```text
Physical Disk
     │
     ▼
Physical Volume
     │
     ▼
Volume Group
     │
     ▼
LVM Thin Pool
     │
     ├── VM Disk
     ├── VM Disk
     ├── Container Root Disk
     └── VM Disk
```

The physical disk provides the underlying capacity.

The physical volume makes the disk available to LVM.

The volume group provides the storage pool.

The thin pool provides thin-provisioned block storage.

VM2Cloud allocates guest disks from the thin pool.

---

# Thin Provisioning

With thin provisioning, the guest disk's configured size does not necessarily equal the amount of physical storage immediately consumed.

For example:

```text
Thin Pool Physical Capacity: 500 GB

VM Disk Configured Size:     200 GB
Actual Data Consumed:         20 GB
```

The VM sees a 200 GB virtual disk, while the physical storage consumed may initially be much smaller.

As the guest writes additional data, additional blocks are allocated from the thin pool.

This makes capacity planning particularly important.

---

# LVM-Thin Storage Characteristics

| Feature                    | LVM-Thin  |
| -------------------------- | --------- |
| Storage level              | Block     |
| VM disk images             | Supported |
| Container root directories | Supported |
| Image format               | Raw       |
| Thin provisioning          | Supported |
| Snapshots                  | Supported |
| Clones                     | Supported |
| Shared between nodes       | No        |
| Local storage              | Yes       |

The underlying storage documentation confirms that LVM-Thin supports raw VM images, snapshots, clones, and both VM image and container root-directory content, but is not shared storage.

---

# Snapshots

LVM-Thin supports storage-level snapshots.

Snapshots can be used to preserve a point-in-time state of a guest disk before making changes.

Typical use cases include:

* Testing configuration changes.
* Testing software upgrades.
* Creating a rollback point.
* Preparing a VM before major configuration changes.

Snapshots should not replace regular backups.

---

# Clones

LVM-Thin supports efficient cloning.

A clone can be created from an existing VM or template where the VM2Cloud workflow supports the operation.

Cloning is useful when deploying multiple similar virtual machines.

Because clones depend on the underlying storage capabilities, verify that the source VM and target storage meet the required conditions before performing the operation.

---

# Capacity Management

Thin provisioning does not create additional physical storage.

For example:

```text
Physical Thin Pool:       100 GB

VM 1 configured disk:      80 GB
VM 2 configured disk:      80 GB
VM 3 configured disk:      80 GB

Total configured:         240 GB
Physical capacity:        100 GB
```

This configuration represents over-provisioning.

Although the guests may initially consume less physical storage than their configured sizes, the thin pool can eventually become full.

> **Warning:** If an LVM-Thin pool becomes full, guest I/O operations can fail and workloads can experience filesystem or data-integrity problems. Monitor thin-pool capacity carefully.

---

# Thin Pool Metadata

An LVM-Thin pool uses metadata in addition to the data area.

Both the data capacity and metadata capacity must be monitored.

A metadata-full condition can affect thin-provisioned volumes even when the data area still has available capacity.

For production environments, monitor both the data and metadata utilization of the thin pool.

---

# Resizing the Thin Pool

If additional physical capacity is available, an LVM-Thin pool can be extended.

The underlying platform documentation notes that when extending a thin pool, the metadata pool must also be considered and extended appropriately.

Before resizing:

1. Confirm the volume group has free space.
2. Confirm the correct thin pool.
3. Confirm that the additional physical capacity is available.
4. Ensure that the operation is supported by the current storage configuration.
5. Back up important workloads.
6. Perform the resize.
7. Verify both data and metadata capacity afterward.

Do not resize an LVM-Thin pool blindly.

---

# Verification

After adding LVM-Thin storage:

1. Select **Datacenter**.
2. Open **Storage**.
3. Locate the LVM-Thin storage.
4. Confirm that it is enabled.
5. Confirm that the correct node is available.
6. Verify the configured content types.
7. Check the available storage capacity.
8. Confirm that the expected volume group is used.
9. Confirm that the expected thin pool is used.

For a functional test:

1. Create or select a test VM.
2. Open **Hardware**.
3. Add a virtual disk.
4. Select the LVM-Thin storage.
5. Configure the required disk size.
6. Complete the operation.
7. Verify that the disk appears in the VM hardware list.
8. Start the VM if appropriate.
9. Verify that the guest operating system detects the disk.

### Screenshot 6

```text
[ Place Screenshot Here — LVM-Thin Storage Verification ]
```

---

# Common Issues

## Thin Pool Is Not Available

Possible causes include:

* The volume group does not exist.
* The thin pool does not exist.
* The thin pool is inactive.
* The underlying disk is unavailable.
* The thin pool is already assigned to another storage configuration.

### Resolution

1. Verify the physical disk.
2. Verify the volume group.
3. Verify the thin pool.
4. Check its available capacity.
5. Reopen the LVM-Thin storage configuration.
6. Retry the operation.

---

## Thin Pool Is Full

A full thin pool can cause guest I/O failures.

### Resolution

1. Check which guests are consuming the storage.
2. Review the thin-pool data usage.
3. Review metadata usage.
4. Remove unnecessary data only after verifying that it is no longer required.
5. Expand the thin pool if suitable free space exists in the volume group.
6. Review capacity planning to prevent recurrence.

> **Warning:** Do not simply continue allocating virtual disks when the thin pool is close to full.

---

## Thin Pool Metadata Is Full

If thin-pool metadata reaches its limit, storage operations may fail even if the data pool still has free space.

### Resolution

1. Check thin-pool metadata usage.
2. Determine whether metadata can be safely extended.
3. Verify free space in the volume group.
4. Extend the metadata area according to the supported LVM procedure.
5. Verify the thin pool after the operation.

---

## VM Disk Cannot Be Created

Check:

1. The storage is enabled.
2. The storage is available on the selected node.
3. **Disk image** content is enabled.
4. The thin pool is available.
5. The thin pool has sufficient capacity.
6. The VM has the required permissions.
7. The storage is not in an error state.

---

## Container Root Disk Cannot Be Created

Check:

1. The storage is enabled.
2. The storage is available on the node.
3. **Container** content is enabled.
4. The thin pool is available.
5. Sufficient capacity exists.
6. The user has the required permissions.

---

## LVM-Thin Storage Is Not Available on Another Node

This is expected when the thin pool is local to one node.

LVM-Thin pools cannot be shared directly between multiple VM2Cloud nodes.

If VM workloads need shared storage for cluster-wide access or migration requirements, use an appropriate shared-storage technology instead.

---

## Snapshot Operation Fails

Check:

1. The guest disk is located on LVM-Thin storage.
2. The storage is online.
3. Sufficient thin-pool capacity exists.
4. Thin-pool metadata has sufficient capacity.
5. The VM or container is in a supported state for the requested operation.

---

# Best Practices

* Monitor thin-pool data usage continuously.
* Monitor thin-pool metadata usage.
* Avoid excessive over-provisioning.
* Maintain sufficient free capacity.
* Use backups in addition to snapshots.
* Use meaningful storage IDs.
* Keep local LVM-Thin storage assigned only to nodes that can access it.
* Test snapshot and clone workflows before using them for production recovery procedures.
* Plan thin-pool expansion before capacity becomes critical.
* Keep sufficient free space in the underlying volume group.
* Do not remove or recreate a thin pool while dependent guest volumes exist.
* Document which node owns each local LVM-Thin storage pool.

---

# Related Documentation

* [Disks Overview](Disks-Overview.md)
* [View Disk Information](View-Disk-Information.md)
* [Disk Management](Disk-Management.md)
* [LVM](LVM.md)
* [ZFS](ZFS.md)
* [Directory](Directory.md)
* [Disk Troubleshooting](Disk-Troubleshooting.md)
* [Storage Overview](../../02-Datacenter/Storage/Storage-Overview.md)
* [Add Storage](../../02-Datacenter/Storage/Add-Storage.md)

---

# Summary

LVM-Thin provides VM2Cloud with thin-provisioned block storage for virtual machines and containers.

It uses an existing LVM volume group and thin pool and supports efficient snapshots and clones. Unlike shared storage technologies, an LVM-Thin pool is local to the node on which its underlying storage exists.

The most important operational requirement is **capacity management**. Thin provisioning allows configured virtual disk sizes to exceed currently consumed physical capacity, but the thin pool must never be allowed to unexpectedly run out of data or metadata space.

Always monitor the thin pool, maintain backups, and verify the underlying volume group and thin pool before making storage changes.
