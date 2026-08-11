# ZFS

---

## Overview

**ZFS** is a storage technology that combines filesystem and storage-pool management.

VM2Cloud can use a local ZFS pool as storage for virtual machines and containers. ZFS provides features such as:

* Storage pools.
* Filesystems and datasets.
* Data integrity checks.
* Snapshots.
* Clones.
* Optional compression.
* Optional thin provisioning for virtual volumes.

The VM2Cloud ZFS storage backend uses the underlying ZFS functionality of the platform. A local ZFS storage can provide VM disk images and container root directories. It is local storage and is not shared automatically between cluster nodes.

---

## When to Use

Use ZFS when you need:

* Local storage for VM2Cloud nodes.
* ZFS storage pools.
* VM disk images backed by ZFS.
* Container storage backed by ZFS.
* Storage-level snapshots.
* Storage-level clones.
* ZFS data-integrity features.
* Optional compression.
* Optional thin provisioning.
* Multiple physical disks combined into a ZFS pool.

---

## Prerequisites

Before creating or managing ZFS:

* Log in to VM2Cloud with sufficient permissions.
* Ensure the target node is online.
* Ensure the required physical disks are detected.
* Confirm that the selected disks do not contain required data.
* Decide the required ZFS pool layout.
* Ensure sufficient disk capacity is available.
* For production systems, use an appropriate redundancy layout.
* Back up important data before destructive disk operations.

> **Warning:** Creating a ZFS pool can erase or reinitialize the selected disks. Verify every selected disk before creating the pool.

---

# Procedure

## Step 1: Select the Node

1. Log in to the VM2Cloud web interface.
2. Locate the resource tree.
3. Select the node containing the disks that will be used for ZFS.
4. Wait for the node interface to load.

### Screenshot 1

```text
[ Place Screenshot Here — Select Node ]
```

---

## Step 2: Open Disks

1. In the node navigation menu, select **Disks**.
2. Review the available physical disks.
3. Identify the disks that will be used for the ZFS pool.
4. Verify their device identifiers and capacities.

### Screenshot 2

```text
[ Place Screenshot Here — Node → Disks ]
```

---

## Step 3: Verify the Physical Disks

Before creating the pool:

1. Identify each target disk.
2. Check the disk capacity.
3. Check the disk model and identifying information.
4. Confirm that the disks are not required by the operating system.
5. Confirm that the disks are not already assigned to another storage configuration.
6. Confirm that no required VM or container data exists on them.

Do not continue if the disk identity or data state is uncertain.

### Screenshot 3

```text
[ Place Screenshot Here — Physical Disk List ]
```

---

## Step 4: Open ZFS Management

1. In the node's **Disks** section, open the ZFS management area.
2. Review existing ZFS pools.
3. Confirm whether a suitable pool already exists.
4. If a new pool is required, select the option to create a ZFS pool.

The underlying platform provides a local ZFS pool backend called `zfspool`. It can use a local ZFS pool or a ZFS filesystem within a pool.

### Screenshot 4

```text
[ Place Screenshot Here — ZFS Management ]
```

---

## Step 5: Configure the ZFS Pool

Enter the required ZFS pool configuration.

The exact available options depend on the VM2Cloud version and the selected pool layout.

Configure:

1. Pool name.
2. RAID level or ZFS virtual device layout.
3. Required physical disks.
4. Additional supported options, where available.
5. Review the selected disks.
6. Confirm that the pool layout provides the required capacity and redundancy.
7. Create the pool.

### Screenshot 5

```text
[ Place Screenshot Here — Create ZFS Pool ]
```

---

## Step 6: Confirm Pool Creation

1. Review the confirmation dialog.
2. Verify the pool name.
3. Verify every selected disk.
4. Verify the selected RAID/layout type.
5. Confirm that the operation may modify or erase the selected disks.
6. Confirm the operation.
7. Wait for the task to complete.

### Screenshot 6

```text
[ Place Screenshot Here — ZFS Pool Confirmation ]
```

---

## Step 7: Verify the ZFS Pool

After creation:

1. Refresh the ZFS page.
2. Locate the newly created pool.
3. Verify the pool name.
4. Verify the pool status.
5. Verify the expected disks are members of the pool.
6. Verify the pool capacity.
7. Confirm that no error is reported.

### Screenshot 7

```text
[ Place Screenshot Here — ZFS Pool Successfully Created ]
```

---

# Adding ZFS as VM2Cloud Storage

Creating a ZFS pool and adding that pool as VM2Cloud storage are related but separate operations.

The ZFS pool provides the underlying storage.

The VM2Cloud storage definition makes that pool available to VM and container management operations.

---

## Step 8: Open Datacenter Storage

1. Select **Datacenter** in the resource tree.
2. Open **Storage**.
3. Click **Add**.
4. Select **ZFS** where the storage-type option is provided.

The underlying storage manager identifies the local ZFS storage type as `zfspool`.

### Screenshot 8

```text
[ Place Screenshot Here — Datacenter → Storage → Add → ZFS ]
```

---

## Step 9: Configure ZFS Storage

Configure the required storage properties.

Typical properties include:

* Storage ID.
* ZFS pool/filesystem.
* Content types.
* Node availability.
* Optional storage settings supported by the current VM2Cloud version.

### Screenshot 9

```text
[ Place Screenshot Here — ZFS Storage Configuration ]
```

---

## Step 10: Select the ZFS Pool

1. Select the ZFS pool or filesystem that will be used.
2. Verify that it is the pool created in the previous steps.
3. Do not select a pool containing unrelated production data unless that use has been planned.
4. Confirm the selected pool.

The ZFS storage backend uses the configured pool/filesystem as the location where storage allocations are created.

---

## Step 11: Select Content Types

Select the content types required by the storage.

For local ZFS storage, the supported content includes:

* **Disk images** for VMs.
* **Container root directories**.

The underlying ZFS backend supports `images` and `rootdir`.

### Screenshot 10

```text
[ Place Screenshot Here — ZFS Content Types ]
```

---

## Step 12: Configure Node Availability

If the storage is local:

1. Select the node that can access the ZFS pool.
2. If the VM2Cloud cluster contains multiple nodes, verify that the pool actually exists on every node where the storage is configured as available.
3. Do not mark local ZFS storage as available on nodes that cannot access it.

Local ZFS storage is not shared storage. The underlying storage documentation lists local ZFS as `Shared: no`.

---

## Step 13: Add the Storage

1. Review the complete configuration.
2. Verify the storage ID.
3. Verify the selected ZFS pool.
4. Verify content types.
5. Verify node availability.
6. Click **Add**.
7. Wait for the task to finish.

### Screenshot 11

```text
[ Place Screenshot Here — Add ZFS Storage ]
```

---

# Configuration / Options

## Storage ID

The **Storage ID** is the name used by VM2Cloud to identify the ZFS storage.

Use a meaningful name.

Example:

```text
local-zfs
```

---

## Pool

The **Pool** identifies the ZFS pool or filesystem used by the VM2Cloud storage backend.

Example:

```text
tank
```

The backend can also use a ZFS filesystem within the pool.

Example:

```text
tank/vmdata
```

The underlying documentation recommends using an additional ZFS filesystem for VM images when appropriate.

---

## Content

The content configuration determines what can be stored on the ZFS storage.

For local ZFS, supported content includes:

* **Disk images**
* **Container root directories**

VM disk images are stored using ZFS-backed raw volumes, while container storage uses ZFS subvolumes.

---

## Nodes

The **Nodes** setting determines which cluster nodes can use the storage configuration.

For local ZFS:

* The underlying ZFS pool must actually be accessible from the node.
* The storage should not be configured as available on unrelated nodes.
* A cluster configuration does not automatically make local ZFS shared.

---

## Disable

The storage can be disabled when it should temporarily be unavailable for normal VM2Cloud storage operations.

Disabling storage does not delete the underlying ZFS pool.

---

# ZFS Pool Layouts

When creating a ZFS pool, the disk layout determines redundancy, usable capacity, and failure tolerance.

The appropriate layout depends on the number of disks and the workload.

Common ZFS layouts include:

| Layout | General Characteristics                        |
| ------ | ---------------------------------------------- |
| Stripe | Maximum usable capacity but no disk redundancy |
| Mirror | Data is mirrored between disks                 |
| RAIDZ1 | Single-disk parity                             |
| RAIDZ2 | Two-disk parity                                |
| RAIDZ3 | Three-disk parity                              |

The correct layout must be selected according to the production requirements.

Do not select a layout only because it provides the largest usable capacity.

---

# ZFS Mirror

A mirror stores copies of data on multiple disks.

Example:

```text
Disk 1 ──┐
         ├── ZFS Mirror
Disk 2 ──┘
```

A mirror provides redundancy and can continue operating after the failure of one disk in a two-disk mirror.

---

# RAIDZ

RAIDZ provides parity-based redundancy.

For example:

```text
Disk 1 ─┐
Disk 2 ─┤
Disk 3 ─┼── RAIDZ
Disk 4 ─┘
```

The usable capacity is lower than the total raw disk capacity because space is used for parity and filesystem overhead.

---

# ZFS Datasets

A ZFS pool can contain datasets.

Example:

```text
tank
├── vmdata
├── backup
└── containers
```

Datasets can have their own ZFS properties.

The VM2Cloud storage backend can use a ZFS filesystem inside a pool rather than using the entire pool directly.

---

# ZFS Compression

ZFS supports compression.

Compression can reduce the amount of physical storage required for compressible data.

Compression should be evaluated according to:

* Workload.
* CPU capacity.
* Data compressibility.
* Storage performance requirements.

When supported by the environment, compression can be configured on the relevant ZFS dataset.

The underlying documentation demonstrates enabling compression on a dedicated VM dataset.

---

# ZFS Sparse Volumes

The local ZFS backend supports sparse provisioning.

A sparse volume does not reserve the complete virtual size immediately.

For example:

```text
Configured VM disk:   200 GB
Initially allocated:   20 GB
```

As data is written, additional physical storage is consumed.

Sparse provisioning must be monitored carefully because the underlying pool can become full.

The underlying ZFS backend exposes the `sparse` option for this purpose.

---

# ZFS Block Size

The ZFS storage backend supports a configurable block-size parameter.

Block size affects how data is stored and can influence storage efficiency and performance.

Do not change block-size settings without understanding the workload and ZFS behavior.

Use the platform defaults unless a specific storage requirement justifies changing them.

---

# ZFS Snapshots

ZFS provides native snapshot functionality.

Snapshots preserve a point-in-time state of ZFS data.

VM2Cloud can use ZFS snapshot functionality for supported guest storage.

Common uses include:

* Testing changes.
* Creating rollback points.
* Protecting a VM before an upgrade.
* Creating a temporary recovery point.

Snapshots are not a replacement for backups.

---

# ZFS Clones

ZFS supports efficient clones.

A clone can be created from a snapshot and can be used when deploying similar workloads.

Cloning can reduce the amount of storage initially required compared with creating a completely independent copy.

The underlying ZFS storage backend supports cloning.

---

# ZFS Storage Characteristics

| Feature                    | Local ZFS     |
| -------------------------- | ------------- |
| Storage level              | File          |
| VM images                  | Supported     |
| Container root directories | Supported     |
| VM image format            | Raw           |
| Container format           | ZFS subvolume |
| Snapshots                  | Supported     |
| Clones                     | Supported     |
| Shared storage             | No            |
| Sparse provisioning        | Supported     |
| ZFS datasets               | Supported     |

The underlying storage documentation lists local ZFS as non-shared storage with snapshot and clone support.

---

# Verification

After creating the ZFS pool:

1. Open **Node → Disks → ZFS**.
2. Confirm that the pool is listed.
3. Confirm that its status is healthy.
4. Confirm the expected disks are members.
5. Verify the available capacity.

After adding ZFS as VM2Cloud storage:

1. Select **Datacenter**.
2. Open **Storage**.
3. Select the ZFS storage.
4. Confirm that it is enabled.
5. Confirm the expected node is available.
6. Confirm the selected content types.
7. Verify the storage capacity.

### Functional Verification

1. Create a test VM.
2. Open the VM's **Hardware** section.
3. Add a test disk.
4. Select the ZFS storage.
5. Complete the disk creation.
6. Confirm that the disk appears in the VM hardware list.
7. Start the test VM.
8. Verify that the guest operating system can access the disk.

### Snapshot Verification

1. Select the test VM.
2. Open **Snapshots**.
3. Create a test snapshot.
4. Wait for the task to complete.
5. Verify that the snapshot appears.
6. Confirm that the VM remains operational.

### Clone Verification

1. Use a suitable test VM or template.
2. Start the clone operation.
3. Select the ZFS storage where required.
4. Complete the operation.
5. Verify that the new VM is created.
6. Confirm that the cloned VM can start.

---

# Common Issues

## ZFS Pool Creation Fails

Possible causes include:

* Disk is already in use.
* Existing ZFS metadata exists.
* Disk is unavailable.
* Insufficient disks for the selected layout.
* Hardware or controller problems.
* Incorrect disk selection.

### Resolution

1. Verify every selected disk.
2. Confirm that the disks are not required by another storage configuration.
3. Check disk availability.
4. Verify that enough disks are available for the selected layout.
5. Review the VM2Cloud task output.
6. Correct the underlying issue.
7. Retry only after confirming that the disks are safe to modify.

---

## ZFS Pool Is Not Visible

Possible causes include:

* Pool was not created successfully.
* Pool is unavailable.
* The node cannot access the disks.
* ZFS configuration has an error.

### Resolution

1. Refresh the **Disks → ZFS** page.
2. Verify the node status.
3. Check physical disk availability.
4. Review the task history.
5. If required, use CLI verification to inspect ZFS pool state.

---

## Storage Is Unavailable on Another Node

Local ZFS is not shared storage.

A ZFS pool physically located on one node cannot automatically be used by another node.

### Resolution

Use an appropriate shared-storage technology when multiple nodes require simultaneous access.

Do not simply configure the storage as available on another node without verifying actual storage access.

---

## ZFS Pool Is Full

A full ZFS pool can cause storage operations and guest workloads to fail.

### Resolution

1. Check pool usage.
2. Identify large datasets or volumes.
3. Remove unnecessary data only after verifying it is no longer required.
4. Review snapshots.
5. Remove unnecessary snapshots where appropriate.
6. Expand the pool if additional disks can safely be added.
7. Review storage capacity planning.

> **Warning:** Do not allow production ZFS pools to reach 100% utilization.

---

## ZFS Snapshot Uses More Space Than Expected

Snapshots preserve blocks that are still referenced by the snapshot.

If data changes after creating snapshots, the old blocks may remain allocated.

### Resolution

1. Review existing snapshots.
2. Identify snapshots that are no longer required.
3. Remove obsolete snapshots according to the VM2Cloud snapshot workflow.
4. Monitor pool usage after cleanup.

---

## VM Disk Cannot Be Created

Check:

1. ZFS storage is enabled.
2. The storage is available on the selected node.
3. **Disk image** content is enabled.
4. The ZFS pool is healthy.
5. Sufficient pool capacity exists.
6. The user has sufficient permissions.

---

## Container Storage Cannot Be Created

Check:

1. ZFS storage is enabled.
2. The storage is available on the selected node.
3. **Container** content is enabled.
4. The ZFS pool is healthy.
5. Sufficient capacity exists.
6. The user has sufficient permissions.

---

# CLI Verification

The web UI should be used for normal ZFS management.

CLI should only be used when additional verification or troubleshooting is required.

## List ZFS Pools

```bash
zpool list
```

Use this command to review available ZFS pools and their capacity.

---

## Check Pool Status

```bash
zpool status
```

Use this command to identify:

* Pool health.
* Disk state.
* Read errors.
* Write errors.
* Checksum errors.
* Resilver activity.

---

## List ZFS Filesystems

```bash
zfs list
```

This displays ZFS pools, datasets, and their usage.

---

## Scan ZFS Storage

The underlying storage manager provides:

```bash
pvesm zfsscan
```

This can be used to identify available ZFS filesystems when troubleshooting storage configuration.

> **Note:** These commands are verification tools. Do not modify or destroy ZFS pools from the CLI unless the operation is specifically required and the administrator understands its consequences.

---

# Best Practices

* Select the correct ZFS layout before creating the pool.
* Never create a pool on disks containing required data.
* Use redundancy for production storage.
* Monitor pool capacity regularly.
* Monitor pool health and disk errors.
* Keep backups independent of ZFS snapshots.
* Use snapshots for short-term rollback, not long-term backup.
* Use meaningful pool and dataset names.
* Consider compression for suitable workloads.
* Avoid allowing pools to reach full capacity.
* Monitor the effect of snapshots on available capacity.
* Keep local ZFS storage restricted to nodes that can access the pool.
* Test storage operations before placing production workloads on a new pool.
* Document the physical disks belonging to each production ZFS pool.

---

# Related Documentation

* [Disks Overview](Disks-Overview.md)
* [View Disk Information](View-Disk-Information.md)
* [Disk Management](Disk-Management.md)
* [LVM](LVM.md)
* [LVM-Thin](LVM-Thin.md)
* [Directory](Directory.md)
* [Disk Troubleshooting](Disk-Troubleshooting.md)
* [Storage Overview](../../02-Datacenter/Storage/Storage-Overview.md)
* [Add Storage](../../02-Datacenter/Storage/Add-Storage.md)
* [ZFS Storage](../../02-Datacenter/Storage/Storage-Types.md)

---

# Summary

ZFS provides VM2Cloud with a powerful local storage backend that supports VM disks, container storage, snapshots, and clones.

A typical configuration consists of:

```text
Physical Disks
      ↓
   ZFS Pool
      ↓
ZFS Dataset / Filesystem
      ↓
VM2Cloud ZFS Storage
      ↓
VMs / Containers
```

ZFS is local storage and is not automatically shared between cluster nodes.

Before creating a ZFS pool, carefully verify the physical disks and select an appropriate redundancy layout. After creation, verify pool health, capacity, storage availability, and guest operations.

For production environments, combine ZFS snapshots with an independent backup strategy and continuously monitor pool health and capacity.
