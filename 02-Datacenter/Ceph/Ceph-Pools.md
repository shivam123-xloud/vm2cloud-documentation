# Ceph Pools

---

## Overview

A **pool** is a logical division of Ceph storage. Guest disks live in a pool, and the pool's settings determine how many copies of that data exist and how it is distributed.

Creating a pool is not the last step. A Ceph pool becomes usable by guests only once it is added as VM2Cloud **storage** — the pool holds the data, the storage entry makes it selectable when creating a disk.

For Ceph as a whole, see [Ceph Overview](Ceph-Overview.md). For the components underneath, see [Monitors and OSDs](Ceph-Monitors-and-OSDs.md).

---

## When to Use

Create or manage pools when you need to:

* Make Ceph capacity available to guests for the first time.
* Separate workloads with different durability requirements.
* Provide CephFS file storage as well as block storage.
* Adjust replication settings.
* Review how capacity is being consumed.

Most clusters need **one pool** for guest disks. Create more only when a workload genuinely needs different settings — extra pools divide capacity and complicate placement without helping.

---

## Prerequisites

* Ceph is deployed, with monitors in quorum and OSDs `up` and `in`.
* Cluster health is `HEALTH_OK`.
* You know the durability required — see size and min_size below.
* You have enough raw capacity for the replication factor.

---

# Understanding Pool Settings

## Size and min_size

These two numbers decide durability and availability.

| Setting | Meaning |
|---|---|
| **size** | How many copies of each object exist. |
| **min_size** | How many copies must be available for the pool to accept writes. |

| Configuration | Node failures survived | Writes continue after | Usable capacity |
|---|---|---|---|
| size 3 / min_size 2 | 1 | 1 failure | ~33% of raw |
| size 2 / min_size 2 | 0 for writes | none | ~50% of raw |
| size 2 / min_size 1 | 1 | 1 failure | ~50% of raw |

**Use size 3 / min_size 2.**

> **Warning:** size 2 / min_size 1 looks attractive — it survives a node failure and doubles usable capacity. It is also how Ceph clusters lose data. With one copy remaining and writes still being accepted, a second failure during recovery loses everything written since the first. The capacity saving is not worth it.

## Placement Groups

Objects are grouped into **placement groups** (PGs), and CRUSH places PGs onto OSDs. The PG count affects how evenly data spreads.

Too few PGs means uneven distribution. Too many wastes memory and CPU on every OSD.

Modern Ceph manages this automatically through the PG autoscaler, which adjusts the count as the cluster grows. Leave it enabled unless you have a specific reason not to.

> **Verify:** Confirm whether the PG autoscaler is enabled by default in this deployment,
> and capture the PG-related fields in the pool creation dialog.

---

# Procedure

## Step 1: Open the Pools Panel

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** in the resource tree.
3. Expand **Ceph**.
4. Click **Pools**.

Existing pools are listed with their size, min_size, PG count, and usage.

---

### Screenshot 1

**Ceph Pools Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Ceph → Pools, showing at least one pool with its size,
> min_size, PG count, and usage, plus the **Create**, **Edit**, and **Destroy** controls.

---

## Step 2: Create a Pool

1. Click **Create**.
2. Enter a **Name**.
3. Set **Size** to `3`.
4. Set **Min. Size** to `2`.
5. Leave the PG autoscaler enabled.
6. Optionally tick the option to add it as VM2Cloud storage automatically.
7. Click **Create**.

If the interface offers to create the storage entry for you, take it — it saves the separate step below and gets the settings right.

---

### Screenshot 2

**Create Pool Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create Pool dialog, showing Name, Size, Min. Size, PG settings, and
> any option to add the pool as storage.

---

## Step 3: Add the Pool as Storage

Skip this if the previous step created it automatically.

1. Select **Datacenter** → **Storage**.
2. Click **Add** and select the RBD storage type.
3. Enter an **ID** for the storage entry.
4. Select the Ceph **Pool**.
5. Set the **Content** types — normally disk image and container.
6. Set which **Nodes** may use it, or leave it available to all.
7. Confirm.

See [Add Storage](../Storage/Add-Storage.md) for the full dialog.

---

### Screenshot 3

**Adding the Pool as Storage**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Storage dialog with the RBD type selected, showing the pool
> selector and content type checkboxes.

---

## Step 4: Verify With a Test Guest

1. Create a virtual machine with its disk on the new storage.
2. Start it.
3. Migrate it to another node.
4. Confirm the migration completes quickly, without transferring the disk.

That last point is the whole purpose of shared storage. If migration still copies the disk, the guest is not on Ceph.

---

## Step 5: Monitor Usage

Watch pool usage over time, and remember two things:

* Every gigabyte written consumes three, with size 3.
* Ceph needs free space to re-replicate after a failure.

Keep overall cluster usage below **80%**. Above that, a node failure may leave nowhere to put the recovered copies, and the cluster stays degraded until capacity is added.

---

### Screenshot 4

**Pool Usage**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Pools panel showing usage figures, alongside the Ceph overview
> capacity summary.

---

## Step 6: Destroy a Pool

> **Warning:** Destroying a pool **permanently deletes every guest disk stored in it**. This cannot be undone and there is no recovery. Confirm nothing is using the pool before proceeding.

1. Migrate or delete every guest with disks on the pool.
2. Remove the corresponding VM2Cloud storage entry.
3. Select the pool.
4. Click **Destroy**.
5. Confirm.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Name** | Pool identifier. |
| **Size** | Number of copies. Use `3`. |
| **Min. Size** | Copies required to accept writes. Use `2`. |
| **PG autoscale mode** | Whether Ceph manages the placement group count. Leave enabled. |
| **CRUSH rule** | Which placement rule applies. The default spreads copies across hosts. |
| **Add as Storage** | Creates the VM2Cloud storage entry automatically. |

> **Verify:** Capture the complete Create Pool dialog and confirm the exact field labels
> and defaults in this deployment.

---

# Verification

Verify the following:

* The pool appears with size 3 and min_size 2.
* Cluster health is `HEALTH_OK`.
* A corresponding storage entry exists and is enabled.
* The storage is selectable when creating a guest disk.
* A test guest runs from the pool.
* That guest migrates between nodes without copying its disk.
* Pool usage is consistent with the replication factor.
* Overall cluster usage is below 80%.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Pool not selectable when creating a disk | No storage entry exists for it, or the content type does not include disk images. |
| Pool is read-only | Available copies fell below min_size. Restore failed OSDs. |
| Capacity consumed faster than expected | size 3 means three times the written data. This is expected. |
| Cluster degraded and will not recover | Insufficient free space to re-replicate. Add capacity. |
| Uneven usage across OSDs | Capacity is unbalanced across nodes, or the PG count is too low. |
| Migration still copies the disk | The guest's disk is on local storage, not the Ceph pool. |
| Cannot destroy a pool | Guests still have disks on it, or the storage entry still exists. |
| Warning about PG counts | Let the autoscaler settle. Investigate if it persists. |

---

# Best Practices

- **size 3 / min_size 2.** Treat this as fixed.
- One pool for guest disks is enough for most clusters. Extra pools fragment capacity.
- Let the interface create the storage entry when it offers.
- Leave the PG autoscaler enabled.
- Plan capacity as raw ÷ 3, and stay below 80% used.
- Verify shared storage by migrating a guest and confirming the disk does not move.
- Monitor usage as a trend — Ceph needs headroom to recover, not just to store.
- Back up guests on Ceph. It protects against hardware failure, not deletion. See [Backup Jobs Overview](../Backup/Backup-Jobs-Overview.md).
- Remove guests and the storage entry before destroying a pool, so the destroy is uneventful.

---

# Related Documentation

- [Ceph Overview](Ceph-Overview.md)
- [Ceph Monitors and OSDs](Ceph-Monitors-and-OSDs.md)
- [Node Ceph](../../03-Nodes/Node-Ceph.md)
- [Add Storage](../Storage/Add-Storage.md)
- [Manage Storage](../Storage/Manage-Storage.md)
- [Storage Types](../Storage/Storage-Types.md)
- [Backup Jobs Overview](../Backup/Backup-Jobs-Overview.md)
- [Migrate Virtual Machine](../../04-Virtual-Machines/Migrate-Virtual-Machine.md)

---

# Summary

A Ceph pool holds guest disks, and its size and min_size settings decide how many copies exist and when writes stop. Use **size 3 / min_size 2** — the capacity saved by reducing it is the same capacity that keeps your data alive during a failure.

A pool is not usable until it is added as VM2Cloud storage; the pool holds the data, the storage entry makes it selectable. Verify the result by migrating a guest between nodes and confirming its disk does not move — that is what shared storage buys you. And keep the cluster below 80% full, because Ceph needs free space to recover, not merely to store.
