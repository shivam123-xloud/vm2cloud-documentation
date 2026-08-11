# Backup Jobs Overview

---

## Overview

A **backup job** is a scheduled, cluster-wide task that backs up selected virtual machines and containers automatically on a recurring schedule.

This is distinct from backing up a single guest by hand. A manual backup protects one guest, once, when someone remembers. A backup job protects a defined set of guests, on a schedule, without anyone remembering.

Backup jobs are configured at **Datacenter → Backup** and can span every node in the cluster.

### Backup is not replication

The two are often confused, and they solve different problems.

| | Backup | [Replication](../Replication/Replication-Overview.md) |
|---|---|---|
| Produces | Independent restore points kept over time | A current copy of the guest on another node |
| Protects against | Deletion, corruption, ransomware, mistakes made days ago | Node failure |
| Retention | Multiple versions, kept for a defined period | Only the latest synchronized state |
| Restores to | Any retained point in time | The state at the last sync |

Replication cannot recover a file deleted last week — the deletion is replicated too. Backup can. Production workloads generally need both.

---

## When to Use

Use backup jobs when:

* Production guests need protection without manual intervention.
* A retention policy must be applied consistently.
* Backups should run outside business hours.
* A compliance requirement calls for scheduled, retained backups.
* Multiple guests across several nodes need the same schedule.

Use a manual backup instead for a one-off snapshot before a risky change. See [Backup and Restore VM](../../04-Virtual-Machines/Backup-and-Restore-VM.md).

---

## Prerequisites

Before configuring backup jobs, ensure that:

* You have administrator privileges.
* A storage exists with backup content enabled. See [Add Storage](../Storage/Add-Storage.md).
* That storage has enough free capacity for the retention you intend to keep.
* You know which guests must be protected.
* You know the acceptable backup window.
* The cluster has quorum.

---

# How Backup Jobs Work

## Selection

A job defines *which* guests it covers. Selection can be by explicit list, by node, by pool, or by "all guests" — and the choice matters over time. A job covering "all guests" automatically picks up newly created guests; a job with an explicit list does not.

This is the most common cause of an unprotected guest: it was created after the backup job was defined, and the job only covered a fixed list.

## Backup Modes

Each job runs in one of three modes, trading consistency against downtime.

| Mode | Guest state | Consistency | Downtime |
|---|---|---|---|
| **Snapshot** | Stays running | Crash-consistent unless the guest agent quiesces it | None |
| **Suspend** | Paused during backup | Better than snapshot | Brief pause |
| **Stop** | Shut down, backed up, restarted | Fully consistent | Full duration of the backup |

**Snapshot** is the usual choice for production. It requires storage that supports snapshots.

A crash-consistent backup is equivalent to pulling the power cord: the filesystem recovers on boot, but an application mid-write may need its own recovery. For databases, either use **Stop** mode or ensure the guest agent is installed and configured to quiesce the filesystem.

## Retention

Retention controls how many backups are kept and for how long. Without it, backup storage fills and jobs begin to fail.

See [Backup Retention](Backup-Retention.md).

## Execution

At the scheduled time, the job runs on each node holding a selected guest. Backups are written to the configured storage, and the result appears in the task log.

A job that fails partway may leave some guests backed up and others not. Always check job results rather than assuming success.

---

# The Backup Panel

Datacenter → Backup lists every configured job.

Each row typically shows the schedule, selected guests, target storage, mode, retention, and whether the job is enabled.

---

### Screenshot 1

**Datacenter Backup Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Backup, showing the job list with the **Add**, **Edit**,
> **Remove**, **Run now**, and **Job Detail** controls visible.

---

### Screenshot 2

**Backup Job Detail**

```text
[ Place Screenshot Here ]
```

> **Capture:** The detail view for a single backup job, showing which guests it covers
> and its current configuration.

---

# Configuration / Options

Settings that define a backup job. See [Create Backup Job](Create-Backup-Job.md) for the procedure.

| Option | Description |
|---|---|
| **Node** | Restricts the job to guests on one node, or covers all nodes. |
| **Storage** | Where backups are written. Must have backup content enabled. |
| **Schedule** | When the job runs. |
| **Selection mode** | How guests are chosen — all, by node, by pool, or an explicit list. |
| **Send email to** | Address for job reports. |
| **Email notification** | Whether to notify always or only on failure. |
| **Compression** | Compression algorithm applied to the backup. |
| **Mode** | Snapshot, Suspend, or Stop. |
| **Enable** | Whether the job is active. |
| **Retention** | How many backups to keep, and for how long. |

> **Verify:** Capture the complete Add Backup Job dialog and confirm the exact field
> labels, the available compression algorithms, and the notification options.

---

# Verification

Verify the following:

* The job appears in the Datacenter → Backup list.
* The job is enabled.
* The next scheduled run time is correct.
* Every guest that should be protected is covered by a job.
* A manual run completes successfully.
* Backup files appear on the target storage.
* Retention is trimming old backups as intended.
* Email reports arrive if configured.
* Storage free space is stable over several cycles.

Confirm coverage by listing your guests and checking each one appears in a job — not by assuming the job's selection is still correct.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| A guest is not being backed up | The job uses an explicit list and the guest was created afterwards. Add it, or switch to a selection mode that includes new guests automatically. |
| Job fails with insufficient space | Retention is keeping too many backups, or the storage is undersized. Reduce retention or add capacity. |
| Backup storage full | No retention policy is configured. See [Backup Retention](Backup-Retention.md). |
| Snapshot mode fails | The guest's storage does not support snapshots. Use Suspend or Stop mode, or move the guest to snapshot-capable storage. |
| Database inconsistent after restore | Snapshot mode is crash-consistent. Use Stop mode or configure the guest agent to quiesce the filesystem. |
| Job runs but some guests are skipped | Check the task log for per-guest errors — a locked or migrating guest may be skipped. |
| Backups take longer than the window | Stagger jobs, enable compression, or reduce how many guests run in one job. |
| No email reports | Check the notification setting and that the cluster can send mail. |
| Job did not run | The job is disabled, or the schedule expression does not mean what was intended. |

---

# Best Practices

- **Test a restore.** A backup that has never been restored is an assumption, not a protection. Restore to a scratch guest periodically.
- Prefer a selection mode that picks up new guests automatically, so a newly created guest is protected by default.
- Always configure retention. Storage exhaustion is the most common cause of backup failure.
- Schedule backups outside business hours, and stagger jobs so they do not all start at once.
- Keep backups on storage separate from the guests they protect. A backup on the same disk protects against nothing.
- Enable failure notifications at minimum. Silent failure is the worst outcome.
- Use Stop mode, or a properly configured guest agent, for databases and other transactional workloads.
- Review job results weekly rather than trusting the schedule.
- Document which guests are covered, by which job, with what retention.
- Run backup **and** replication for critical workloads. They protect against different things.

---

# Related Documentation

- [Create Backup Job](Create-Backup-Job.md)
- [Manage Backup Job](Manage-Backup-Job.md)
- [Backup Retention](Backup-Retention.md)
- [Backup and Restore VM](../../04-Virtual-Machines/Backup-and-Restore-VM.md)
- [Backup and Restore Container](../../05-Containers/Backup-and-Restore-Container.md)
- [Replication Overview](../Replication/Replication-Overview.md)
- [Storage Overview](../Storage/Storage-Overview.md)
- [Add Storage](../Storage/Add-Storage.md)

---

# Summary

Backup jobs protect selected guests automatically on a schedule, across the whole cluster, with a retention policy controlling how much history is kept. They differ fundamentally from replication: replication keeps a current copy for fast recovery from node failure, while backup keeps independent restore points that can recover from deletion, corruption, or a mistake made days ago.

The two failure modes that matter most are a guest silently falling outside a job's selection, and backup storage filling because no retention was configured. Check coverage explicitly, always set retention, and test a restore before you need one.
