# VM Task History

---

## Overview

The **Task History** tab lists operations that have been performed on this virtual machine — starts, stops, migrations, backups, snapshots, configuration changes — with who ran each one, when, and whether it succeeded.

It is the first place to look when something happened to a machine and you need to know what, when, and who.

The node-level [Task History](../03-Nodes/Task-History.md) covers the same information for everything on a node. This tab filters it to one machine, which is usually what you want when investigating a specific guest.

---

## When to Use

Open Task History when you need to:

* Find out why a machine stopped or restarted.
* Confirm a backup or snapshot actually ran.
* See who performed an operation.
* Read the output of a failed task.
* Confirm a migration completed.
* Establish a timeline after an incident.
* Verify a scheduled job affected this machine.

---

## Prerequisites

* You have permission to view the virtual machine.
* The machine exists on a node that is online. Task history is stored per node.

---

# Procedure

## Step 1: Open the Task History Tab

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select the virtual machine.
4. Click **Task History**.

Tasks are listed with the most recent first.

---

### Screenshot 1

**VM Task History Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine → Task History, showing several tasks of different types
> with their start time, end time, user, description, and status.

---

## Step 2: Read the Task List

| Column | Meaning |
|---|---|
| **Start Time** | When the task began. |
| **End Time** | When it finished. Empty while still running. |
| **Node** | The node the task ran on. |
| **User name** | The account that initiated it. Scheduled jobs show the account they run as. |
| **Description** | What the task did, for example `VM 100 - Start` or `VM 100 - Backup`. |
| **Status** | `OK` for success, or an error message. |

A task with no end time is still running. A task showing an error kept its output — open it.

---

## Step 3: Open a Task to See Its Output

1. Double-click the task, or select it and open it.
2. Read the **Output** tab for the full log.
3. Check the **Status** tab for the result.

The output is where the actual reason for a failure appears. The status column tells you a backup failed; the output tells you the target storage was full.

---

### Screenshot 2

**Task Output**

```text
[ Place Screenshot Here ]
```

> **Capture:** A task detail window opened from VM Task History, showing the Output tab
> with log lines and the Status tab available.

---

## Step 4: Investigate a Timeline

When establishing what happened to a machine:

1. Find the last known-good point.
2. Read forward through the tasks in order.
3. Note who ran each one.
4. Look for a failed task immediately before the symptom appeared.
5. Cross-check against the node-level [Task History](../03-Nodes/Task-History.md) for events affecting the whole node.

A machine that stopped with no corresponding stop task was not stopped by anyone — look at the node instead. A node reboot, an HA action, or a host crash will not appear in the guest's own history as a deliberate stop.

---

# Configuration / Options

Task History is a read-only view.

| Control | Purpose |
|---|---|
| **Task filter** | Narrow the list by task type or status. |
| **Refresh** | Reload the list. |
| Opening a task | Show its full output and result. |

> **Verify:** Capture the VM Task History tab and confirm the available filter controls
> and how far back the retained history goes in this deployment.

---

# Verification

Verify the following:

* Recent operations you performed appear in the list.
* Scheduled backups affecting this machine appear.
* Task status matches what you expect.
* Failed tasks show usable output.
* The user shown matches who actually performed the operation.
* Timestamps align with when the events occurred — if they do not, check time synchronization on the node. See [Time and NTP](../03-Nodes/System/Time-and-NTP.md).

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| A task is missing | It may have run against the node rather than the guest. Check the node's [Task History](../03-Nodes/Task-History.md). |
| History is shorter than expected | Task history is retained per node and rotates. Older entries are removed. |
| Machine stopped with no stop task | It was not stopped through the interface. Check the node history for a reboot, an HA action, or a crash. |
| Task shows an error with no useful output | Open the task and read the full Output tab rather than the status column. |
| Timestamps look wrong | Node time may be out of sync. See [Time and NTP](../03-Nodes/System/Time-and-NTP.md). |
| History empty after migration | Task history is stored on the node the task ran on. Check the previous node. |
| Cannot see the tab | Confirm you have permission to view the machine. |

---

# Best Practices

- Check Task History before assuming a machine failed on its own. Most unexplained state changes have a task behind them.
- Read the task **output**, not just the status column, when investigating a failure.
- Cross-check the node history when the guest history has no explanation.
- Use the user column to establish who did what during an incident review.
- Confirm scheduled backups appear here rather than trusting the job configuration alone.
- Check node time synchronization if timestamps ever look inconsistent — a timeline built on wrong clocks is worse than none.

---

# Related Documentation

- [Task History](../03-Nodes/Task-History.md) — node-level view
- [Task Log and Cluster Log](../01-Getting-Started/Task-Log-and-Cluster-Log.md)
- [VM Summary](VM-Summary.md)
- [Manage Virtual Machine](Manage-Virtual-Machine.md)
- [VM Troubleshooting](VM-Troubleshooting.md)
- [Backup and Restore VM](Backup-and-Restore-VM.md)
- [CT Task History](../05-Containers/CT-Task-History.md)

---

# Summary

Task History lists every operation performed on a virtual machine, with the user who ran it, the time, and the result. It is the first place to look when establishing what happened to a machine and when.

Two things are worth remembering. The status column tells you a task failed; the task **output** tells you why, so open it. And a machine that changed state with no corresponding task was not changed through the interface — check the node-level history for a reboot, an HA recovery, or a host failure.
