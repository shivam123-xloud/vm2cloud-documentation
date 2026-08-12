# Container Summary

---

## Overview

The **Summary** tab is the default view for a container — the first screen shown when you select one in the resource tree.

It reports whether the container is running, which node it is on, how much CPU and memory it is using, and holds the **Notes** panel for per-container documentation.

Everything here is read-only. Summary reports state; the other tabs change it.

For the virtual machine equivalent, see [VM Summary](../04-Virtual-Machines/VM-Summary.md). The container view is similar but simpler, because a container has no guest agent and no separate operating system reporting back.

---

## When to Use

Open the Summary tab when you need to:

* Check whether a container is running.
* See which node it is on.
* Check CPU, memory, and swap usage.
* Check its HA state.
* Review resource usage over time before resizing.
* Read or update the notes attached to it.

---

## Prerequisites

* You have permission to view the container.
* The container exists on a node that is online.

---

# Procedure

## Step 1: Open the Summary Tab

1. Log in to the VM2Cloud web interface.
2. Expand the node in the resource tree.
3. Select the container.
4. Click **Summary**.

The Summary tab opens by default when a container is selected.

---

### Screenshot 1

**Container Summary Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A running container → Summary, showing the status panel, the Notes panel,
> and the usage graphs below.

---

## Step 2: Read the Status Panel

| Field | What it tells you |
|---|---|
| **Status** | `running` or `stopped`. |
| **HA State** | Whether the container is managed by High Availability. `none` means it is not an HA resource. |
| **Node** | Which cluster node the container is on. |
| **CPU usage** | Current usage against the assigned cores. |
| **Memory usage** | Current usage against the configured limit. |
| **Swap usage** | Swap consumed against the configured swap. |
| **Bootdisk size** | Size of the root disk. |
| **IPs** | Addresses configured on the container's interfaces. |

Unlike a virtual machine, a container reports its addresses without needing an agent — the host can see the container's network configuration directly.

> **Verify:** Capture the container Summary status panel and confirm the exact field
> names and whether IP addresses are shown here in this deployment.

---

### Screenshot 2

**Status Panel Detail**

```text
[ Place Screenshot Here ]
```

> **Capture:** The status panel of a running container, showing status, node, CPU,
> memory, swap, and any address information.

---

## Step 3: Watch Swap Usage

Swap is worth attention on containers in a way it is not on virtual machines.

A container has both a memory limit and a swap limit, set on the [Resources](Manage-Container-Resources.md) tab. When the workload exceeds its memory limit, it begins using swap. Sustained swap usage means the container is under-provisioned, and performance will suffer well before anything fails outright.

If swap usage is consistently above zero on a container that should have headroom, increase its memory rather than its swap.

---

## Step 4: Review the Usage Graphs

Below the status panel are graphs for CPU, memory, network traffic, and disk I/O over time.

Use the time-range control to change the window, and the **Maximum** / **Average** control to switch aggregation.

**Average** is better for spotting trends. **Maximum** is better for capacity decisions, because a container averaging 40% memory while touching its limit each afternoon needs more memory — and the average hides that.

---

### Screenshot 3

**Usage Graphs**

```text
[ Place Screenshot Here ]
```

> **Capture:** The container Summary tab scrolled to the usage graphs, with the
> time-range selector and the Maximum / Average control visible.

---

## Step 5: Use the Notes Panel

The **Notes** panel holds free text attached to this container.

1. Click the edit control on the Notes panel.
2. Enter or update the text.
3. Save.

Record what the container is for, who owns it, what depends on it, and anything the next administrator should know. Notes travel with the container.

Notes support Markdown formatting.

> **Verify:** Confirm the exact edit control on the Notes panel for containers.

---

### Screenshot 4

**Notes Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Notes panel on a container Summary tab, in edit mode.

---

# Configuration / Options

Summary is a read-only view. The only editable element is the Notes panel.

| Control | Purpose |
|---|---|
| **Time range** | Window covered by the graphs. |
| **Maximum / Average** | How graph values are aggregated. |
| **Notes edit** | Free-text documentation attached to the container. |
| **Tags** (in the header) | Labels applied to the container. |

Action buttons in the header — **Start**, **Shutdown**, **Migrate**, **Console**, **More** — are covered in [Manage Container](Manage-Container.md).

---

# Verification

Verify the following:

* The Summary tab loads and shows the current status.
* Status matches what you expect.
* The node shown is the node you expect, particularly after a migration.
* CPU, memory, and swap figures look plausible.
* Swap usage is at or near zero for a well-provisioned container.
* Graphs render and show recent data.
* HA state matches the intended configuration.
* Notes are present and current.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Graphs are empty | The container was recently created or started. Data accumulates over time. |
| Graphs stopped updating | The container may be stopped, or the node's statistics service may not be running. See [Services](../03-Nodes/System/Services.md). |
| Swap usage is consistently high | The container is short of memory. Increase the memory limit on [Resources](Manage-Container-Resources.md) rather than the swap limit. |
| Memory usage sits at the limit | Same cause. The workload needs more memory than it has been given. |
| HA State shows `none` unexpectedly | The container is not registered as an HA resource. See [HA Resources](../02-Datacenter/HA/HA-Resources.md). |
| Node shown is not the expected one | The container was migrated, or HA recovered it. Check [Task History](../03-Nodes/Task-History.md). |
| No addresses shown | Check the container's [Network](CT-Network.md) configuration. |
| Summary will not load | The node may be offline. See [Node Troubleshooting](../03-Nodes/Node-Troubleshooting.md). |
| Notes cannot be edited | Confirm you have permission to modify the container. |

---

# Best Practices

- Treat sustained swap usage as an under-provisioning signal, not a normal operating state.
- Use the **Maximum** aggregation before resizing memory — averages hide the peaks that matter.
- Use the Notes panel to record purpose, owner, and dependencies.
- Check the node field after any HA event.
- Use Tags alongside Notes so containers can be grouped and filtered.
- Review usage graphs periodically rather than only when something is slow.

---

# Related Documentation

- [Container Overview](Container-Overview.md)
- [Manage Container](Manage-Container.md)
- [Manage Container Resources](Manage-Container-Resources.md)
- [Container Options](CT-Options.md)
- [CT Network](CT-Network.md)
- [Container Console](Container-Console.md)
- [Container Troubleshooting](Container-Troubleshooting.md)
- [HA Resources](../02-Datacenter/HA/HA-Resources.md)
- [VM Summary](../04-Virtual-Machines/VM-Summary.md)

---

# Summary

The Summary tab is the default view for a container and the quickest way to establish its state: running or stopped, which node it is on, and how much CPU, memory, and swap it is consuming. It also holds the Notes panel for per-container documentation.

Unlike a virtual machine, a container needs no guest agent for the host to see its configuration, so its addresses and resource usage are reported directly. The figure worth watching is **swap** — sustained swap usage means the container is short of memory, and the fix is more memory rather than more swap.
