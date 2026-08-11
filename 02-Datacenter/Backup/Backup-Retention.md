# Backup Retention

---

## Overview

**Retention** controls how many backups a job keeps and for how long. Backups outside the policy are pruned automatically when the job next runs.

Retention is the setting that decides whether your backup storage stays healthy or fills up and takes the backups down with it. A job with no retention policy accumulates backups forever until the storage is full, at which point every job writing to that storage begins to fail.

It is also the setting that decides how far back you can recover. Keeping only the last backup means a problem discovered two days late is unrecoverable — the corruption has already been backed up over the good copy.

Retention is configured per job, on the job's settings. See [Create Backup Job](Create-Backup-Job.md) and [Manage Backup Job](Manage-Backup-Job.md).

---

## When to Use

Review retention when:

* Creating any backup job.
* Backup storage is filling.
* A compliance requirement specifies a retention period.
* Recovery requirements change.
* Backup jobs begin failing for space.
* Adding guests to an existing job.

---

## Prerequisites

Before configuring retention, ensure that:

* You have administrator privileges.
* You know how far back recovery must be possible.
* You know any compliance-mandated retention period.
* You know the size of a typical backup and the free space available.
* You understand that reducing retention deletes existing backups.

---

# How Retention Works

## Pruning Happens at Run Time

Retention is applied when the job runs, not continuously. Reducing retention does not immediately delete anything — the pruning happens on the next run.

This means a job that is disabled or failing will not prune. Storage can fill even with a sensible policy configured, because the job that would have pruned it never ran.

## Keep Rules Are Layered

The keep options work together rather than as alternatives. A backup is retained if **any** rule still needs it.

With `keep-daily = 7` and `keep-monthly = 6`, you get the last seven daily backups plus one backup per month for six months. A single backup can satisfy more than one rule.

This layering is what gives you fine detail for recent history and coarse coverage for older history, without keeping everything.

## Retention Is Per Job

Each job prunes only its own backups. Several jobs writing to one storage each keep their own history, and the storage must hold the sum of all of them.

This surprises people: retention looks conservative on each job, but five jobs on one storage means five sets of retained backups.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Keep Last** | Keep the most recent *n* backups regardless of age. |
| **Keep Hourly** | Keep one backup per hour for the last *n* hours. |
| **Keep Daily** | Keep one backup per day for the last *n* days. |
| **Keep Weekly** | Keep one backup per week for the last *n* weeks. |
| **Keep Monthly** | Keep one backup per month for the last *n* months. |
| **Keep Yearly** | Keep one backup per year for the last *n* years. |
| **Keep All** | Keep every backup. No pruning. Requires manual management. |

> **Warning:** **Keep All**, or leaving every keep option unset, means backups accumulate indefinitely. The storage will eventually fill and every job writing to it will start failing. Only use it when something else is managing deletion.

> **Verify:** Capture the retention section of the backup job dialog and confirm the
> exact option labels and which are available in this deployment.

---

### Screenshot 1

**Retention Options**

```text
[ Place Screenshot Here ]
```

> **Capture:** The retention section of the Add or Edit Backup Job dialog, showing every
> keep option and its input field.

---

# Example Policies

## Standard production

```text
Keep Daily:    7
Keep Weekly:   4
Keep Monthly:  6
```

A week of daily restore points, a month of weekly ones, and half a year of monthly ones. Roughly 17 backups retained per guest, covering same-day mistakes through to problems discovered months later.

## Minimal, space-constrained

```text
Keep Last:     3
```

Only the three most recent backups. Cheap, but a problem discovered after three backup cycles is unrecoverable. Acceptable for guests that are easy to rebuild, not for data you cannot recreate.

## Compliance-driven

```text
Keep Daily:    30
Keep Monthly:  12
Keep Yearly:   7
```

A month of daily detail, a year of monthly, seven years of annual. Sized for a formal retention obligation. Plan storage capacity carefully before applying this.

## Frequently changing workload

```text
Keep Hourly:   24
Keep Daily:    7
Keep Monthly:  3
```

For guests backed up several times a day where recent granularity matters most.

---

# Estimating Storage

Before applying a policy, estimate what it needs:

```text
storage required  ≈  backups retained  ×  average backup size  ×  guests in job
```

Then add the other jobs writing to the same storage, and leave headroom. Backups grow as guests grow, and a storage running at 95% has no room for the next backup to be written before the old one is pruned.

A reasonable target is to keep the storage below 80% under the full retention policy.

---

# Verification

Verify the following:

* The retention policy is set on every backup job.
* Backup counts on the target storage match the policy after a few cycles.
* Old backups are being pruned, not accumulating.
* Free space on backup storage is stable rather than trending down.
* The oldest available backup is old enough to meet your recovery requirement.
* Compliance-mandated periods are actually satisfied by the configured rules.

Check free space over several cycles. A single reading tells you nothing about the trend.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Storage fills despite a retention policy | The job is disabled or failing, so pruning never runs. Pruning only happens when the job runs. |
| More backups retained than expected | Keep rules layer. A backup satisfying any rule is kept. |
| Older backups disappeared unexpectedly | Retention was reduced, and the next run pruned them. |
| Storage still full after reducing retention | Pruning happens on the next run. Trigger the job manually. |
| Cannot recover from a problem found last week | Retention is too short. Add weekly or monthly rules. |
| Storage fills even though each job looks conservative | Retention is per job. Several jobs on one storage each keep their own history. |
| Backup fails for space with free space showing | There is not enough room to write the new backup before the old one is pruned. Increase headroom. |
| Compliance period not met | Check the keep rules actually cover the required span, and that pruning has not been shortening it. |

---

# Best Practices

- **Set retention when you create the job**, never as a later cleanup task.
- Layer rules — daily for recent detail, weekly and monthly for depth.
- Size storage for the full policy across every job writing to it, plus headroom.
- Keep the storage below 80% under the complete policy.
- Monitor free space as a trend, not a snapshot.
- Never use **Keep All** unless something else deletes old backups.
- Confirm you do not need the restore points before reducing retention.
- Match retention to how long a problem might realistically go unnoticed, not just to the last known-good state.
- Document the retention policy for each job and why it was chosen.
- Re-check the policy whenever guests are added to a job.

---

# Related Documentation

- [Backup Jobs Overview](Backup-Jobs-Overview.md)
- [Create Backup Job](Create-Backup-Job.md)
- [Manage Backup Job](Manage-Backup-Job.md)
- [Storage Overview](../Storage/Storage-Overview.md)
- [Manage Storage](../Storage/Manage-Storage.md)
- [Storage Troubleshooting](../Storage/Storage-Troubleshooting.md)

---

# Summary

Retention decides both how much storage your backups consume and how far back you can recover. The keep rules layer, so a policy of daily, weekly, and monthly gives fine detail recently and coarse coverage further back without keeping everything.

Two things catch people out. Pruning only happens when the job runs, so a disabled or failing job lets storage fill even with a sensible policy configured. And retention is per job, so several jobs sharing one storage each keep their own history — the storage must hold the sum. Size for the full policy, keep headroom, and watch free space as a trend.
