# Storage Content Browser

---

## Overview

Selecting a storage in the resource tree opens its own set of tabs, showing what is actually stored on it and how much space is left.

This is where you go to see the **files** — backups, ISO images, container templates, disk images — rather than the storage's configuration. It is also where you restore an individual backup, or delete files to reclaim space.

The tabs available depend on what content types the storage is configured to hold:

| Tab | Shows |
|---|---|
| **Summary** | Capacity, used space, and status |
| **Backups** | Backup files, with restore and delete actions |
| **ISO Images** | Uploaded installation media |
| **CT Templates** | Container base images |
| **Import** | Disk images available to import. See [Storage Import](Storage-Import.md) |
| **Permissions** | Who can use this storage. See [Storage Permissions](Storage-Permissions.md) |

For configuring the storage itself, see [Manage Storage](Manage-Storage.md). For what each storage type can hold, see [Storage Types](Storage-Types.md).

---

## When to Use

Open a storage's content view when you need to:

* Check how much free space remains.
* Find a specific backup file.
* Restore a single backup, including to a different guest ID.
* Delete old backups or unused ISO images to reclaim space.
* Confirm an upload or download completed.
* See which guests have disks on this storage.
* Investigate why a storage is full.

---

## Prerequisites

* You have permission to view the storage.
* The storage is enabled and reachable from the node you are browsing it on.

---

# Procedure

## Step 1: Open the Storage

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select the storage beneath it.

The storage's tabs appear.

---

### Screenshot 1

**Storage Selected in the Resource Tree**

```text
[ Place Screenshot Here ]
```

> **Capture:** A storage selected under a node, showing its full tab list — Summary,
> Backups, ISO Images, CT Templates, Import, Permissions.

---

## Step 2: Check Capacity on the Summary Tab

1. Click **Summary**.

The Summary tab reports total capacity, used space, and available space, along with usage over time.

Watch the trend rather than the single figure. Storage that has been filling steadily for a month will fill completely at a predictable point, and backups begin failing before it reaches zero — there must be room to write a new backup before the old one is pruned.

---

### Screenshot 2

**Storage Summary Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A storage → Summary, showing capacity, used space, and the usage graph.

---

## Step 3: Browse Backups

1. Click **Backups**.

Each backup file is listed with its guest, creation date, format, and size.

| Column | Meaning |
|---|---|
| **Name** | The backup file, including the guest ID and timestamp. |
| **Date** | When it was taken. |
| **Format** | Compression and archive format. |
| **Size** | Space consumed. |
| **Notes** | Any comment recorded with the backup. |

This is the list your retention policy is pruning. If it is longer than expected, see [Backup Retention](../Backup/Backup-Retention.md) — retention only runs when the job runs.

---

### Screenshot 3

**Backups Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A storage → Backups, showing several backup files with the **Restore**,
> **Remove**, **Show Configuration**, and **Edit Notes** controls.

---

## Step 4: Restore a Backup

1. Select the backup file.
2. Click **Restore**.
3. Set the target **Storage** for the restored guest.
4. Set the **VM ID** — this may be the original, or a different one.
5. Review the remaining options.
6. Confirm.

Restoring to a **different VM ID** is how you test a backup without touching the original. This is worth doing regularly; a backup that has never been restored is an assumption.

> **Warning:** Restoring to an **existing** guest ID overwrites that guest entirely. Its current disks are replaced by the backup's contents, and the current state is lost. Check the ID before confirming.

---

### Screenshot 4

**Restore Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Restore dialog opened from a backup file, showing the target storage
> and VM ID fields.

---

## Step 5: Inspect a Backup Before Restoring

1. Select the backup.
2. Click **Show Configuration**.

This displays the guest configuration captured in the backup — its hardware, disks, and settings — without restoring anything. Use it to confirm you have the right backup, and to see what the guest looked like at that point.

---

### Screenshot 5

**Backup Configuration**

```text
[ Place Screenshot Here ]
```

> **Capture:** The configuration view of a backup file, showing the captured guest
> settings.

---

## Step 6: Browse ISO Images and Container Templates

1. Click **ISO Images** or **CT Templates**.

These list the installation media and container base images available on this storage. Uploading and downloading them is covered in [Upload Content](Upload-Content.md).

Both accumulate. Old ISO images in particular tend to sit unnoticed for years, and they are usually the easiest space to reclaim.

---

## Step 7: Delete Content to Reclaim Space

1. Select the file.
2. Click **Remove**.
3. Confirm.

> **Warning:** Deletion is immediate and permanent. Deleting a backup removes a restore point — confirm another exists and that retention has not already pruned the alternatives. Deleting an ISO or template does not affect guests already built from it.

---

# Configuration / Options

The content view is mostly read-only. Available actions depend on the tab.

| Action | Available on | Purpose |
|---|---|---|
| **Restore** | Backups | Restore a backup to a chosen storage and guest ID. |
| **Show Configuration** | Backups | Inspect a backup's captured configuration without restoring. |
| **Edit Notes** | Backups | Annotate a backup file. |
| **Remove** | All content tabs | Delete the file permanently. |
| **Upload** / **Download from URL** | ISO Images, CT Templates | Add content. See [Upload Content](Upload-Content.md). |

> **Verify:** Capture each content tab and confirm the exact action buttons available in
> this deployment.

---

# Verification

Verify the following:

* The Summary tab reports plausible capacity and usage.
* Free space is sufficient for the configured backup retention.
* Backup files exist for the guests you expect.
* Backup dates match the schedule of the jobs writing here.
* A test restore to a spare guest ID completes and boots.
* Deleted files no longer appear and space is released.
* ISO images and templates you expect are present.

That test restore is the only real verification a backup works.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Storage full | Check the Backups tab. Retention may not be pruning, because pruning only runs when the job runs. See [Backup Retention](../Backup/Backup-Retention.md). |
| More backups than retention allows | Several jobs write here, each keeping its own history. Retention is per job. |
| Backup missing for a guest | The guest may fall outside the job's selection. See [Manage Backup Job](../Backup/Manage-Backup-Job.md). |
| Restore fails for space | The target storage lacks room for the restored disks. |
| Restore overwrote the wrong guest | The VM ID field was left at the original. Always check it before confirming. |
| Content tab missing | The storage is not configured for that content type. See [Manage Storage](Manage-Storage.md). |
| Space not released after deleting | Some storage types reclaim space asynchronously. Recheck shortly after. |
| Cannot browse the storage | It may be disabled, or unreachable from this node. See [Storage Troubleshooting](Storage-Troubleshooting.md). |

---

# Best Practices

- Watch free space as a trend, not a snapshot. Backups fail before storage reaches zero.
- Restore a backup to a spare guest ID periodically. Untested backups are assumptions.
- Use **Show Configuration** to confirm you have the right backup before restoring.
- Check the VM ID field every time you restore.
- Clear out old ISO images — usually the easiest space to reclaim.
- Use backup notes to record why a particular backup was kept.
- Keep backups on storage separate from the guests they protect.
- Investigate an unexpectedly long backup list before deleting anything; the cause is usually retention not running.

---

# Related Documentation

- [Storage Overview](Storage-Overview.md)
- [Storage Types](Storage-Types.md)
- [Manage Storage](Manage-Storage.md)
- [Upload Content](Upload-Content.md)
- [Storage Import](Storage-Import.md)
- [Storage Permissions](Storage-Permissions.md)
- [Storage Troubleshooting](Storage-Troubleshooting.md)
- [Backup Retention](../Backup/Backup-Retention.md)
- [Manage Backup Job](../Backup/Manage-Backup-Job.md)
- [Backup and Restore VM](../../04-Virtual-Machines/Backup-and-Restore-VM.md)

---

# Summary

Selecting a storage opens its content view — the files it holds rather than its configuration. The Summary tab reports capacity, and the Backups tab lists backup files with restore, inspect, and delete actions.

The most useful thing here is restoring a backup **to a different guest ID**, which lets you verify a backup works without touching the original. Do that periodically. And when a storage fills unexpectedly, the usual cause is retention not pruning — pruning only happens when the backup job runs, so a disabled or failing job lets files accumulate however sensible the policy looks.
