# Disk Troubleshooting

---

## Overview

Disk problems can affect the VM2Cloud node, storage pools, virtual machines, containers, and running workloads.

Disk-related problems can include:

* A physical disk not being detected.
* A disk becoming unavailable.
* Low or exhausted storage capacity.
* SMART warnings.
* LVM problems.
* LVM-Thin pool exhaustion.
* ZFS pool errors.
* Directory filesystem problems.
* Storage appearing offline.
* VM or container disks becoming inaccessible.
* Storage operations failing.
* Disk read/write errors.
* Filesystem or mount problems.

VM2Cloud provides disk and storage information through the web interface. CLI tools are useful for detailed verification when the UI does not provide enough information.

> **Warning:** Do not remove, wipe, initialize, format, or recreate a disk or storage pool until you have confirmed that it does not contain required data.

---

## When to Use

Use this document when:

* A disk is missing from the VM2Cloud interface.
* A storage pool is unavailable.
* A VM cannot access its disk.
* A container cannot access its storage.
* Storage capacity is unexpectedly low.
* An LVM or LVM-Thin pool reports insufficient space.
* A ZFS pool reports errors.
* A physical disk reports SMART errors.
* Storage operations fail.
* A filesystem is mounted incorrectly.
* A storage device disappears after reboot.
* A disk needs health verification.
* A storage backend reports an error.

---

## Prerequisites

Before troubleshooting:

* Log in to VM2Cloud with sufficient permissions.
* Identify the affected node.
* Identify the affected storage.
* Identify the affected VM or container if applicable.
* Confirm whether the issue affects one workload or multiple workloads.
* Do not perform destructive disk operations before identifying the problem.
* Ensure backups exist before attempting recovery operations.
* For hardware failures, have replacement hardware available when required.

For production systems, record the current state before making changes.

---

# Procedure

## Step 1: Identify the Affected Resource

First determine whether the problem affects:

* Physical disk.
* Disk partition.
* LVM volume group.
* LVM-Thin pool.
* ZFS pool.
* Directory filesystem.
* VM disk.
* Container storage.
* VM2Cloud storage definition.

This distinction is important because the troubleshooting procedure depends on the storage layer.

### Screenshot 1

```text
[ Place Screenshot Here — Node Storage/Disk View ]
```

---

## Step 2: Check the Node Status

1. Log in to VM2Cloud.
2. Select the affected node.
3. Check the node status.
4. Confirm that the node is online.
5. Open the node's disk/storage sections.
6. Check whether the affected disk or storage is visible.

If the node itself is unavailable, resolve the node problem before troubleshooting the storage through the UI.

---

## Step 3: Check the Disks Page

1. Select the affected node.
2. Open **Disks**.
3. Review the physical disk list.
4. Locate the affected disk.
5. Check its device identifier.
6. Check its capacity.
7. Check its model and other available information.
8. Check whether the disk reports an error or warning.

### Screenshot 2

```text
[ Place Screenshot Here — Node → Disks ]
```

---

## Step 4: Check Storage Status

1. Select **Datacenter**.
2. Open **Storage**.
3. Locate the affected storage.
4. Check whether it is enabled.
5. Check whether it is available.
6. Verify the nodes assigned to the storage.
7. Open the storage content view if available.
8. Check whether expected content is visible.

The underlying storage manager provides a common storage configuration and status mechanism for the different storage backends.

### Screenshot 3

```text
[ Place Screenshot Here — Datacenter → Storage ]
```

---

# Common Disk Problems

## Problem 1: Disk Is Not Detected

### Symptoms

The physical disk does not appear under:

**Node → Disks**

### Possible Causes

* Loose or failed cable.
* Disk failure.
* Controller problem.
* BIOS/UEFI configuration.
* Hardware RAID configuration.
* Disk not presented to the operating system.
* Driver problem.
* Power problem.
* Disk was removed or replaced.

### Resolution

1. Confirm that the node is online.
2. Refresh the disk list.
3. Check whether other disks are detected.
4. Verify the disk's physical connection.
5. Check the server's storage controller.
6. Check BIOS/UEFI disk detection if required.
7. Check hardware RAID management if the disk is behind a RAID controller.
8. Check operating-system detection from the CLI.
9. Do not initialize or wipe the disk until its identity is confirmed.

### CLI Verification

```bash
lsblk
```

This displays block devices detected by the operating system.

---

## Problem 2: Disk Has SMART Errors

### Symptoms

A physical disk reports:

* SMART warnings.
* Read errors.
* Write errors.
* Reallocated sectors.
* Failing health status.
* Other hardware-health warnings.

VM2Cloud's underlying platform uses `smartmontools` for local disk health monitoring. The `smartctl` command can display SMART information.

### Resolution

1. Identify the physical disk.
2. Confirm that it is the correct device.
3. Check SMART information.
4. Determine whether errors are increasing.
5. Check whether the disk belongs to a redundant storage pool.
6. Ensure current backups exist.
7. Replace the disk when hardware failure is confirmed.
8. Rebuild or resilver the affected storage according to its storage technology.

### CLI Verification

```bash
smartctl -a /dev/sdX
```

Replace `/dev/sdX` with the actual physical disk.

The official documentation uses `smartctl -a /dev/sdX` to obtain disk SMART information.

> **Warning:** Do not run destructive disk tests or disk replacement procedures against the wrong device.

---

# Problem 3: Storage Is Full

### Symptoms

You may see:

* VM disk creation failures.
* Backup failures.
* Container creation failures.
* Write errors.
* Guest filesystem errors.
* Storage operations failing.

A full storage backend can cause I/O errors for guests using volumes on that storage and may result in filesystem inconsistencies or data corruption.

### Resolution

1. Open **Datacenter → Storage**.
2. Select the affected storage.
3. Check available capacity.
4. Identify large or unnecessary content.
5. Check VM and container disk usage.
6. Check old backup files.
7. Check snapshots where applicable.
8. Remove only data that is confirmed unnecessary.
9. Move workloads to another storage when appropriate.
10. Expand the underlying storage if supported.
11. Verify that free space has increased.

> **Warning:** Do not delete VM disks, backups, snapshots, or containers simply because they consume space. Confirm ownership and retention requirements first.

---

# Problem 4: LVM Storage Is Unavailable

### Symptoms

An LVM storage may be unavailable when:

* The volume group cannot be activated.
* The physical volume is missing.
* The storage device is unavailable.
* The storage configuration references an incorrect volume group.
* The underlying disk has failed.

The LVM backend requires an existing volume group identified by its `vgname` configuration.

### Resolution

1. Check the affected node.
2. Open **Datacenter → Storage**.
3. Select the LVM storage.
4. Verify its configured volume group.
5. Check whether the underlying disk is detected.
6. Verify the volume group from the CLI.
7. Check physical-volume status.
8. Resolve the hardware or LVM problem.
9. Recheck the storage in VM2Cloud.

### CLI Verification

```bash
pvs
```

```bash
vgs
```

```bash
lvs
```

These commands display physical volumes, volume groups, and logical volumes.

---

# Problem 5: LVM-Thin Pool Is Full

### Symptoms

LVM-Thin storage can fail when the thin pool runs out of available data or metadata space.

Possible symptoms include:

* VM disk creation failure.
* VM write errors.
* Snapshot failures.
* Clone failures.
* Storage reporting critically low capacity.

LVM-Thin supports snapshots and clones, but it is local storage and must be monitored for capacity.

### Resolution

1. Open **Datacenter → Storage**.
2. Check the LVM-Thin storage.
3. Check its available capacity.
4. Check the underlying thin pool.
5. Identify unnecessary volumes.
6. Remove unused volumes only after verification.
7. Remove obsolete snapshots where appropriate.
8. Extend the thin pool if additional capacity is available.
9. Recheck pool usage.

### CLI Verification

```bash
lvs
```

For additional detail:

```bash
lvs -a
```

Check the thin-pool data and metadata utilization.

> **Warning:** Do not allow an LVM-Thin pool to become completely exhausted.

---

# Problem 6: ZFS Pool Reports Errors

### Symptoms

A ZFS pool may report:

* DEGRADED.
* FAULTED.
* UNAVAIL.
* Read errors.
* Write errors.
* Checksum errors.
* Resilver activity.

ZFS storage supports snapshots and clones and uses ZFS datasets for VM and container storage.

### Resolution

1. Select the affected node.
2. Open the ZFS/disk management view.
3. Check the affected pool.
4. Identify the affected device.
5. Check pool health.
6. Determine whether a disk has failed.
7. Verify whether the pool is redundant.
8. Replace failed hardware when required.
9. Allow the pool to resilver.
10. Verify that the pool returns to a healthy state.

### CLI Verification

```bash
zpool status
```

Use this command to identify:

* Pool state.
* Device state.
* Read errors.
* Write errors.
* Checksum errors.
* Resilver activity.

---

## Problem 7: ZFS Pool Is DEGRADED

A degraded ZFS pool can continue operating depending on the pool layout, but redundancy has been reduced.

### Resolution

1. Run:

```bash
zpool status
```

2. Identify the failed or degraded device.
3. Confirm the physical disk identity.
4. Verify that the disk actually requires replacement.
5. Replace the failed disk using the appropriate ZFS replacement procedure.
6. Monitor resilvering.
7. Wait for the resilver operation to complete.
8. Verify the final pool status.

> **Warning:** Never replace a disk based only on `/dev/sdX` naming. Device names can change. Confirm the physical disk using stable identifiers and hardware information.

---

# Problem 8: ZFS Pool Is UNAVAIL

### Symptoms

The ZFS pool or one of its devices is unavailable.

### Resolution

1. Check physical disk detection.
2. Check storage-controller status.
3. Check cabling and power.
4. Run:

```bash
zpool status
```

5. Determine which device is unavailable.
6. Confirm whether the device has physically failed.
7. Check whether the pool can be imported if the entire pool is missing.
8. Do not force-import a pool without understanding the consequences.
9. Restore hardware availability.
10. Recheck the pool.

---

# Problem 9: Directory Storage Is Offline

### Symptoms

Directory storage may become unavailable when:

* The filesystem is not mounted.
* The mount point changed.
* The storage device failed.
* Permissions changed.
* A remote filesystem is unavailable.
* The filesystem became read-only.

The Directory backend requires an absolute filesystem path and can use local directories or locally mounted filesystems.

### Resolution

1. Open **Datacenter → Storage**.
2. Select the affected Directory storage.
3. Verify the configured path.
4. Select the affected node.
5. Check whether the filesystem is mounted.
6. Check available space.
7. Check filesystem permissions.
8. Restore the mount if required.
9. Verify storage availability again.

### CLI Verification

```bash
findmnt
```

Check a specific mount:

```bash
findmnt /mnt/storage
```

Check capacity:

```bash
df -h /mnt/storage
```

---

# Problem 10: Filesystem Is Read-Only

### Symptoms

Storage can be visible but write operations fail.

Possible symptoms:

* VM disk creation fails.
* ISO upload fails.
* Backup creation fails.
* Container creation fails.
* Guest writes fail.

### Resolution

1. Identify the affected filesystem.
2. Check whether it is mounted read-only.
3. Check kernel/system logs.
4. Investigate filesystem or hardware errors.
5. Do not immediately remount the filesystem as read-write if hardware errors are present.
6. Repair the underlying problem.
7. Restore normal filesystem access.
8. Verify storage operations.

### CLI Verification

```bash
findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS
```

Look for `ro` in the mount options.

---

# Problem 11: VM Disk Cannot Be Created

### Symptoms

The VM disk creation dialog fails or the disk is not created.

### Resolution

Check the following in order:

1. VM is located on the expected node.
2. Selected storage is online.
3. Storage supports **Disk image** content.
4. Storage has sufficient free space.
5. Storage is not disabled.
6. User has sufficient permissions.
7. Underlying physical storage is healthy.
8. Storage pool is not full.
9. Retry the disk creation.

---

# Problem 12: VM Cannot Start After Storage Failure

### Symptoms

A VM fails to start because its virtual disk cannot be accessed.

### Resolution

1. Select the affected VM.
2. Open **Hardware**.
3. Identify the VM disk.
4. Note the configured storage.
5. Open **Datacenter → Storage**.
6. Check whether that storage is available.
7. Verify the storage on the VM's node.
8. Check the underlying disk/pool.
9. Review the failed start task.
10. Restore storage availability.
11. Retry starting the VM.

Do not remove the VM disk from the configuration unless you have confirmed that the disk is no longer required.

---

# Problem 13: Container Storage Cannot Be Accessed

### Symptoms

A container may fail to start because its root filesystem or mount point is unavailable.

### Resolution

1. Select the container.
2. Check its storage configuration.
3. Identify the affected storage.
4. Verify storage availability.
5. Check the underlying disk or pool.
6. Check whether the mount point exists.
7. Restore storage availability.
8. Retry starting the container.

---

# Problem 14: Storage Content Is Missing

### Symptoms

The storage exists but expected:

* VM disks.
* ISO images.
* Templates.
* Backups.
* Snippets.

are not visible.

### Resolution

1. Verify the correct storage ID.
2. Verify the selected node.
3. Check the configured storage path or pool.
4. Verify the selected content type.
5. Check whether the underlying filesystem or pool is mounted.
6. Check storage permissions.
7. Verify the content from the CLI if required.
8. Do not manually move storage files before understanding the storage layout.

---

# Problem 15: Storage Is Available on the Wrong Node

VM2Cloud cluster configuration is shared, but local storage is physically different on each node.

The underlying storage configuration is distributed through the cluster configuration, while local storage remains physically local to each node.

### Resolution

1. Open **Datacenter → Storage**.
2. Select the affected storage.
3. Check the configured nodes.
4. Identify whether the storage is local or shared.
5. Remove nodes that cannot actually access the storage.
6. If the storage is intended to be shared, configure an appropriate shared storage backend.

Do not mark local storage as shared simply to make it appear available to another node.

---

# Problem 16: Disk Operations Are Very Slow

### Possible Causes

* High storage utilization.
* High I/O load.
* Failing disk.
* RAID rebuild.
* ZFS resilver.
* LVM storage contention.
* Network storage latency.
* Excessive VM disk activity.
* Backup operations.
* Snapshot activity.

### Resolution

1. Check node resource usage.
2. Check storage utilization.
3. Check affected disks.
4. Check ZFS pool status if applicable.
5. Check RAID controller status if applicable.
6. Check running backup or migration tasks.
7. Identify high-I/O workloads.
8. Review disk health.
9. Resolve hardware or capacity problems.

---

# Problem 17: Disk Errors Increase Over Time

Increasing read, write, or checksum errors can indicate an underlying hardware or storage problem.

### Resolution

1. Identify the physical disk.
2. Check SMART information.
3. Check storage-pool health.
4. Check kernel logs.
5. Determine whether the errors are increasing.
6. Ensure backups are current.
7. Replace failing hardware when required.
8. Verify storage health after replacement.

---

# Problem 18: Storage Disappeared After Reboot

### Possible Causes

* Filesystem was not mounted automatically.
* `/etc/fstab` configuration is incorrect.
* Disk identifier changed.
* Network storage was unavailable during boot.
* Storage controller failed to initialize.
* ZFS pool was not imported.
* Hardware failure.

### Resolution

1. Check physical disk detection.
2. Check filesystem mounts.
3. Check storage configuration.
4. Check ZFS pool status if applicable.
5. Check `/etc/fstab` for external filesystems.
6. Verify that the storage path exists.
7. Restore the correct mount/import configuration.
8. Verify the storage from the VM2Cloud UI.

---

# Problem 19: Storage Operation Fails With Permission Error

### Possible Causes

* Incorrect filesystem permissions.
* Incorrect ownership.
* Read-only filesystem.
* Storage path is inaccessible.
* User lacks VM2Cloud permissions.

### Resolution

1. Check the VM2Cloud user's permissions.
2. Check storage permissions.
3. Verify the filesystem path.
4. Check filesystem ownership and permissions.
5. Verify that the storage is writable.
6. Retry the operation.

### CLI Verification

```bash
ls -ld /path/to/storage
```

---

# Problem 20: Hardware RAID Disk Problem

When physical disks are managed by a hardware RAID controller, the operating system may not directly expose individual physical disks.

In this situation:

1. Identify the RAID controller.
2. Open the controller's management interface or vendor utility.
3. Check physical disk health.
4. Check RAID array state.
5. Check failed or predictive-failure disks.
6. Follow the controller vendor's disk replacement procedure.
7. Monitor the RAID rebuild.
8. Verify that the VM2Cloud storage becomes healthy again.

The underlying VM2Cloud platform documentation notes that hardware RAID controllers generally provide their own tools for monitoring the disks and RAID array.

---

# Verification

After troubleshooting, verify the problem at every affected layer.

## Physical Disk Verification

Confirm:

* Disk is detected.
* Disk has the expected capacity.
* Disk health is acceptable.
* No new hardware errors are reported.

---

## Storage Pool Verification

For LVM:

```bash
pvs
vgs
lvs
```

For LVM-Thin:

```bash
lvs
```

For ZFS:

```bash
zpool status
```

---

## Filesystem Verification

For Directory storage:

```bash
findmnt
df -h
```

---

## VM2Cloud Storage Verification

1. Open **Datacenter → Storage**.
2. Select the affected storage.
3. Confirm it is available.
4. Confirm the correct nodes are configured.
5. Confirm expected content is visible.
6. Confirm free capacity is available.

---

## Guest Verification

If the storage problem affected a VM or container:

1. Start the workload.
2. Open its console.
3. Verify that the operating system starts correctly.
4. Verify that expected disks are present.
5. Verify filesystem access.
6. Check application functionality.
7. Monitor the workload for further errors.

---

# CLI Verification

CLI should be used only when the UI does not provide sufficient information.

## List Physical Disks

```bash
lsblk
```

---

## Display Disk Information

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL,SERIAL
```

---

## Check Disk Health

```bash
smartctl -a /dev/sdX
```

The underlying platform uses SMART monitoring through `smartmontools`.

---

## Check LVM

```bash
pvs
```

```bash
vgs
```

```bash
lvs
```

---

## Check LVM-Thin

```bash
lvs -a
```

Check the thin-pool data and metadata utilization carefully.

---

## Check ZFS

```bash
zpool status
```

```bash
zpool list
```

```bash
zfs list
```

---

## Check Filesystems

```bash
df -h
```

---

## Check Mounts

```bash
findmnt
```

---

## Check VM2Cloud Storage Status

```bash
pvesm status
```

The underlying storage manager provides `pvesm status` for storage status verification.

---

## List Storage Content

```bash
pvesm list <STORAGE_ID>
```

Replace `<STORAGE_ID>` with the actual storage ID.

---

# Reading Common Storage States

| State     | Meaning                                         | Recommended Action                        |
| --------- | ----------------------------------------------- | ----------------------------------------- |
| Available | Storage is accessible                           | Continue normal operation                 |
| Offline   | Storage cannot currently be accessed            | Check filesystem, pool, or hardware       |
| Full      | No usable capacity remains                      | Free or extend storage                    |
| Degraded  | Redundancy has been reduced                     | Identify and replace failed component     |
| Faulted   | Storage component or pool has a serious failure | Investigate immediately                   |
| Read-only | Writes are blocked                              | Investigate filesystem or hardware errors |
| Missing   | Expected device/storage cannot be detected      | Check hardware and configuration          |

---

# Best Practices

* Monitor disk health continuously.
* Monitor storage capacity.
* Keep production storage below full utilization.
* Maintain independent backups.
* Use redundant storage layouts for production workloads.
* Verify disk identity before destructive operations.
* Do not wipe disks without confirming their purpose.
* Do not remove VM disks to solve a storage-capacity problem without verification.
* Do not mark local storage as shared when it is not actually shared.
* Investigate SMART errors before they become disk failures.
* Monitor ZFS pools after disk replacement.
* Monitor LVM-Thin data and metadata usage.
* Verify filesystem mounts after reboot.
* Keep storage configuration consistent across cluster nodes.
* Document physical disk locations and storage-pool membership.
* Test backups regularly.
* Keep replacement disks available for critical storage systems.

---

# Emergency Recovery Guidelines

If a production storage system fails:

1. Stop unnecessary changes.
2. Identify the affected storage.
3. Determine whether data is still accessible.
4. Check storage health.
5. Check current backups.
6. Avoid destructive recovery commands.
7. Preserve logs and task output.
8. Identify the failed physical component.
9. Restore redundant storage when possible.
10. Restore workloads from backup if the storage cannot be recovered.
11. Verify guest filesystem integrity.
12. Monitor the environment after recovery.
13. Document the incident and corrective action.

> **Warning:** Do not repeatedly reboot, recreate pools, wipe disks, or force storage imports during an unknown storage failure. These actions can make recovery more difficult or permanently destroy recoverable data.

---

# Related Documentation

* [Disks Overview](01-Disks-Overview.md)
* [View Disk Information](02-View-Disk-Information.md)
* [Disk Management](03-Disk-Management.md)
* [LVM](04-LVM.md)
* [LVM-Thin](05-LVM-Thin.md)
* [ZFS](06-ZFS.md)
* [Directory](07-Directory.md)
* [Storage Overview](../06-Storage/Storage-Overview.md)
* [Storage Troubleshooting](../06-Storage/Storage-Troubleshooting.md)
* [VM Troubleshooting](../04-Virtual-Machines/VM-Troubleshooting.md)
* [Container Troubleshooting](../05-Containers/Container-Troubleshooting.md)

---

# Summary

Disk troubleshooting should start at the lowest affected layer and move upward:

```text
Physical Hardware
       ↓
Physical Disk
       ↓
Partition / Volume
       ↓
LVM / LVM-Thin / ZFS / Filesystem
       ↓
VM2Cloud Storage
       ↓
VM / Container
       ↓
Guest Operating System
       ↓
Application
```

Identify the failing layer before making changes.

For physical disks, check detection and SMART health.

For LVM and LVM-Thin, check physical volumes, volume groups, logical volumes, and pool capacity.

For ZFS, check pool and device health with `zpool status`.

For Directory storage, check filesystem mounts, capacity, permissions, and filesystem health.

For all storage types, verify that sufficient free capacity remains. A completely full storage backend can result in guest I/O errors and potentially cause filesystem inconsistencies or data corruption.

The safest recovery process is to identify the problem first, preserve existing data, verify backups, correct the underlying failure, and then verify the storage and affected workloads.
