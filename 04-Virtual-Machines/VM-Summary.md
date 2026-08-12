# VM Summary

---

## Overview

The **Summary** tab is the default view for a virtual machine — the first screen shown when you select one in the resource tree.

It answers the questions you usually have when you open a guest: is it running, where is it running, how hard is it working, and does anything look wrong. It also holds the **Notes** panel, which is where per-machine documentation belongs.

Everything here is read-only. Summary reports state; the other tabs change it.

---

## When to Use

Open the Summary tab when you need to:

* Check whether a machine is running.
* See which node it is on.
* Check CPU and memory usage.
* Find the machine's IP addresses.
* Check its HA state.
* Review resource usage over time before resizing.
* Read or update the notes attached to it.
* Confirm the guest agent is working.

---

## Prerequisites

* You have permission to view the virtual machine.
* The machine exists on a node that is online.

---

# Procedure

## Step 1: Open the Summary Tab

1. Log in to the VM2Cloud web interface.
2. Expand the node in the resource tree.
3. Select the virtual machine.
4. Click **Summary**.

The Summary tab opens by default when a machine is selected, so this is usually already the view you land on.

---

### Screenshot 1

**VM Summary Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A running virtual machine → Summary, showing the status panel, the Notes
> panel, and the usage graphs below.

---

## Step 2: Read the Status Panel

The status panel at the top left reports the machine's current state.

| Field | What it tells you |
|---|---|
| **Status** | `running`, `stopped`, `paused`, or a transitional state. |
| **HA State** | Whether the machine is managed by High Availability, and its HA status. `none` means it is not an HA resource. |
| **Node** | Which cluster node the machine is currently on. |
| **CPU usage** | Current usage, and how many virtual CPUs are assigned. |
| **Memory usage** | Current usage against the configured amount. |
| **Host memory usage** | What the machine is consuming on the host, which differs from what the guest reports. |
| **Bootdisk size** | Size of the boot disk. |
| **IPs** | Addresses reported by the guest agent. |

The header above the panel shows the machine's ID, name, and node — for example `Virtual Machine 100 (test-vm) on node 'v2c1'` — along with the **Tags** control and the action buttons.

---

## Step 3: Check the IP Addresses

The **IPs** field only reports addresses when the QEMU guest agent is installed inside the guest **and** enabled in [VM Options](VM-Options.md).

Without it, this field shows `No Guest Agent configured` rather than the machine's addresses. That is not a fault — it means VM2Cloud has no way to ask the guest what its addresses are.

If you need IP reporting, see [VM Options](VM-Options.md) for enabling the agent. It also gives you cleaner shutdowns and consistent snapshot backups.

---

### Screenshot 2

**Status Panel Detail**

```text
[ Place Screenshot Here ]
```

> **Capture:** The status panel of a virtual machine that has the guest agent working,
> so the IPs field shows real addresses rather than "No Guest Agent configured".

---

## Step 4: Review the Usage Graphs

Below the status panel are graphs covering CPU usage, memory usage, network traffic, and disk I/O over time.

Use the time-range control to change the window — hour, day, week, month, or year — and the **Maximum** / **Average** control to switch how values are aggregated.

**Average** smooths the line and is better for spotting trends. **Maximum** shows peaks and is better for capacity work, because a machine averaging 30% CPU while hitting 100% during business hours needs more CPU, and the average alone hides that.

---

### Screenshot 3

**Usage Graphs**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Summary tab scrolled to the usage graphs, with the time-range selector
> and the Maximum / Average control visible.

---

## Step 5: Use the Notes Panel

The **Notes** panel sits to the right of the status panel and holds free text attached to this machine.

1. Click the edit control on the Notes panel.
2. Enter or update the text.
3. Save.

This is the right place to record what the machine is for, who owns it, what depends on it, and anything the next administrator would need to know before touching it. The notes travel with the machine and are visible to anyone with access to it.

Notes support Markdown formatting.

> **Verify:** Confirm the exact edit control on the Notes panel and whether Markdown
> rendering is enabled in this deployment.

---

### Screenshot 4

**Notes Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Notes panel on a VM Summary tab, in edit mode, showing the text area
> and save control.

---

# Configuration / Options

Summary is a read-only view. The only editable element is the Notes panel.

| Control | Purpose |
|---|---|
| **Time range** | Window covered by the graphs — hour, day, week, month, year. |
| **Maximum / Average** | How graph values are aggregated. |
| **Notes edit** | Free-text documentation attached to the machine. |
| **Tags** (in the header) | Labels applied to the machine, used for grouping and filtering. |

Action buttons in the header — **Start**, **Shutdown**, **Migrate**, **Console**, **More** — are covered in [Manage Virtual Machine](Manage-Virtual-Machine.md).

> **Verify:** Capture the complete Summary tab and confirm the exact field names in the
> status panel and the available graph time ranges.

---

# Verification

Verify the following:

* The Summary tab loads and shows the current status.
* Status matches what you expect — running, stopped, or paused.
* The node shown is the node you expect, particularly after a migration or HA recovery.
* CPU and memory figures look plausible for the workload.
* Graphs render and show recent data.
* IP addresses appear, if the guest agent is configured.
* HA state matches the intended configuration.
* Notes are present and current.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| IPs show "No Guest Agent configured" | The QEMU guest agent is not installed in the guest or not enabled. See [VM Options](VM-Options.md). |
| Graphs are empty | The machine was recently created or started. Data accumulates over time. |
| Graphs stopped updating | The machine may be stopped, or the node's statistics service may not be running. See [Services](../03-Nodes/System/Services.md). |
| Memory usage looks wrong | Host memory usage and guest-reported memory differ. Without the guest agent, VM2Cloud sees only what the host allocates. |
| HA State shows `none` unexpectedly | The machine is not registered as an HA resource. See [HA Resources](../02-Datacenter/HA/HA-Resources.md). |
| Node shown is not the expected one | The machine was migrated, or HA recovered it after a node failure. Check [Task History](../03-Nodes/Task-History.md). |
| Summary will not load | The node may be offline or unreachable. See [Node Troubleshooting](../03-Nodes/Node-Troubleshooting.md). |
| Notes cannot be edited | Confirm you have permission to modify the machine. |

---

# Best Practices

- Install and enable the guest agent on every machine. IP reporting, clean shutdown, and consistent backups all depend on it.
- Use the Notes panel. A machine whose purpose is undocumented becomes one nobody dares decommission.
- Record the owner and the dependencies in Notes, not just the application name.
- Check the **Maximum** aggregation before resizing CPU or memory — averages hide peaks.
- Review the node field after any HA event, so you know where workloads actually ended up.
- Use Tags alongside Notes so machines can be grouped and filtered in the resource tree.

---

# Related Documentation

- [Virtual Machine Overview](Virtual-Machine-Overview.md)
- [Manage Virtual Machine](Manage-Virtual-Machine.md)
- [VM Options](VM-Options.md)
- [Manage VM Hardware](Manage-VM-Hardware.md)
- [VM Console](VM-Console.md)
- [VM Troubleshooting](VM-Troubleshooting.md)
- [HA Resources](../02-Datacenter/HA/HA-Resources.md)
- [Container Summary](../05-Containers/CT-Summary.md)

---

# Summary

The Summary tab is the default view for a virtual machine and the fastest way to establish its state: whether it is running, which node it is on, how much CPU and memory it is using, its HA state, and its IP addresses. It also holds the Notes panel, which is where per-machine documentation belongs.

Two things depend on the guest agent being installed and enabled: IP address reporting and accurate in-guest memory figures. Without it the Summary tab shows `No Guest Agent configured` and reports only what the host can see from outside.
