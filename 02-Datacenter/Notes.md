# Datacenter Notes

---

## Overview

The **Notes** tab at Datacenter level holds free-text documentation attached to the environment as a whole.

It is the one piece of documentation that is guaranteed to be found. Anything written here is visible to every administrator who opens the interface, without needing to know where a wiki lives or having access to it.

That makes it the right place for the small set of facts someone needs when they open VM2Cloud VE without context: who owns this cluster, where the real documentation lives, who to contact, and anything that would cause damage if not known.

Notes support Markdown formatting.

There are equivalent Notes at [node](../03-Nodes/Node-Notes.md) and guest level. Datacenter Notes are for what applies to the whole environment.

---

## When to Use

Use Datacenter Notes for:

* Who owns and operates this environment.
* Where the full documentation and runbooks live.
* Escalation contacts and on-call arrangements.
* Change-control expectations — maintenance windows, approval requirements.
* Environment identity, so nobody confuses production with staging.
* Warnings that apply cluster-wide.
* Links to monitoring and ticketing.

Do **not** use it for:

* Anything secret. Every administrator can read it.
* Detail belonging to a single node or guest — use their own Notes.
* Long documentation. Link to it instead.

---

## Prerequisites

* You have permission to modify datacenter configuration.
* The cluster has quorum, since notes are stored on the cluster file system.

---

# Procedure

## Step 1: Open the Notes Tab

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter** in the resource tree.
3. Click **Notes**.

---

### Screenshot 1

**Datacenter Notes Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Notes, showing existing content and the edit control.

---

## Step 2: Edit the Notes

1. Click the edit control.
2. Enter or update the text.
3. Save.

---

### Screenshot 2

**Editing Notes**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Datacenter Notes tab in edit mode, showing the text area and save
> control.

---

## Step 3: Use a Consistent Structure

Notes are more useful when every environment follows the same shape. A workable starting point:

```markdown
# Production Cluster — London

**Owner:** Platform Team
**Contact:** platform@example.com
**On-call:** See the on-call rota in the service desk

## Change Control
Maintenance window: Sundays 02:00–06:00
Changes outside the window require approval from the platform lead.

## Documentation
Runbooks: <link>
Network diagram: <link>
Ticketing: <link>

## Warnings
- Nodes v2c1 and v2c2 share a single power feed. Do not schedule
  maintenance on both together.
- The `fast-nvme` storage is reserved for database workloads.
```

Keep it short. Notes that grow into a manual stop being read.

---

## Step 4: Keep Them Current

Stale notes are worse than none, because they are trusted. Review them:

* When ownership or contacts change.
* After adding or removing nodes.
* After any change to maintenance arrangements.
* When a warning no longer applies.

Put a review date in the notes themselves if that helps.

---

# Configuration / Options

| Element | Description |
|---|---|
| **Notes content** | Free text with Markdown formatting. |

> **Verify:** Confirm the exact edit control on the Datacenter Notes tab and whether
> Markdown is rendered in this deployment.

---

# Verification

Verify the following:

* The Notes tab shows the saved content.
* Markdown renders as intended.
* Links work.
* Contact details are current.
* Another administrator can read the notes.
* Nothing secret has been written there.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Notes cannot be saved | Confirm you have permission, and that the cluster has quorum — notes live on the cluster file system. |
| Notes not visible to another administrator | They need permission at datacenter level. See [Assign Permissions](Permissions/Assign-Permissions.md). |
| Markdown not rendering | Check the syntax; confirm rendering is supported in this deployment. |
| Notes lost after a cluster change | They are stored on the cluster file system. Confirm the cluster is healthy. See [Cluster File System](Cluster/Cluster-File-System.md). |
| Notes are out of date | No mechanism enforces this. Add reviewing them to your change process. |
| Someone wrote a credential in the notes | Remove it and rotate the credential. Notes are readable by every administrator. |

---

# Best Practices

- Write the few facts someone needs when they open the interface with no context.
- Include environment identity prominently. "Production" at the top of the notes prevents mistakes.
- Link to detailed documentation rather than reproducing it.
- Record cluster-wide warnings — shared power feeds, reserved storage, fragile dependencies.
- **Never** put credentials, keys, or secrets in Notes.
- Use the same structure across every environment you run.
- Review notes as part of any change that alters ownership, contacts, or topology.
- Keep them short enough that people actually read them.

---

# Related Documentation

- [Node Notes](../03-Nodes/Node-Notes.md)
- [VM Summary](../04-Virtual-Machines/VM-Summary.md) — guest Notes panel
- [CT Summary](../05-Containers/CT-Summary.md) — guest Notes panel
- [Datacenter Summary](Datacenter-Summary.md)
- [Datacenter Options](Options.md)
- [Assign Permissions](Permissions/Assign-Permissions.md)
- [Cluster File System](Cluster/Cluster-File-System.md)

---

# Summary

Datacenter Notes hold free-text documentation for the environment as a whole, visible to every administrator who opens the interface. That reach is what makes them valuable — they are the one place documentation cannot be missed.

Use them for ownership, contacts, change-control expectations, environment identity, and cluster-wide warnings, with links out to fuller documentation. Keep them short, review them when anything changes, and never write a secret there: every administrator can read them.
