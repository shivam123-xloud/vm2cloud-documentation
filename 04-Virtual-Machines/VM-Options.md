# VM Options

---

## Overview

The **Options** tab holds per-machine settings that control how a virtual machine behaves — how it boots, whether it starts with the node, how it shuts down, and various guest-integration and protection settings.

These are distinct from **Hardware**, which defines what the machine *has*. Options define how it *behaves*.

Most options can be changed while the machine is running, but many only take effect at the next start. The page notes which.

---

## When to Use

Open the Options tab when you need to:

* Make a machine start automatically when its node boots.
* Change the boot device order, for example to boot from an ISO.
* Control startup and shutdown ordering across several machines.
* Protect a machine from accidental deletion.
* Enable the guest agent for cleaner shutdowns and IP reporting.
* Change the display or BIOS type.
* Rename the machine.

---

## Prerequisites

Before changing options, ensure that:

* You have administrator privileges, or permissions on the machine.
* You know whether the setting requires a restart.
* You have a maintenance window if a restart is needed.
* You understand the effect on dependent machines when changing start or shutdown order.

---

# Procedure

## Step 1: Open the Options Tab

1. Log in to the VM2Cloud web interface.
2. Expand the node in the resource tree.
3. Select the virtual machine.
4. Click **Options**.

The settings are listed with their current values.

---

### Screenshot 1

**VM Options Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine → Options, showing the complete settings list with
> current values and the **Edit** button.

---

## Step 2: Edit a Setting

1. Select the setting.
2. Click **Edit**.
3. Change the value.
4. Click **OK**.

The list updates immediately. Whether the machine's behaviour changes immediately depends on the setting.

---

### Screenshot 2

**Editing an Option**

```text
[ Place Screenshot Here ]
```

> **Capture:** The edit dialog for a single option — ideally **Start at boot** or
> **Boot Order** — showing the available values.

---

## Step 3: Configure Start at Boot

Determines whether the machine starts automatically when its node boots.

1. Select **Start at boot**.
2. Click **Edit**.
3. Tick to enable.
4. Click **OK**.

Without this, a machine stays off after a node restart until someone starts it manually. For production workloads that is rarely what you want.

---

## Step 4: Configure Start/Shutdown Order

Controls the sequence in which machines start and stop on the node. This matters when machines depend on each other.

1. Select **Start/Shutdown order**.
2. Click **Edit**.
3. Set:
   - **Order** — a number. Lower numbers start first, and shut down last.
   - **Startup delay** — seconds to wait after this machine starts, before starting the next.
   - **Shutdown timeout** — seconds to wait for a clean shutdown before forcing it.
4. Click **OK**.

A database server should start before the application server that depends on it, and shut down after it. Give the database a lower order number and a startup delay long enough for it to accept connections before the next machine starts.

---

### Screenshot 3

**Start and Shutdown Order**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Start/Shutdown order edit dialog, showing the Order, Startup delay,
> and Shutdown timeout fields.

---

## Step 5: Configure Boot Order

Determines which device the machine tries to boot from, and in what sequence.

1. Select **Boot Order**.
2. Click **Edit**.
3. Enable the devices to boot from and set their sequence.
4. Disable devices that should not be tried.
5. Click **OK**.

Set the disk first for normal operation. Put the CD/DVD drive first temporarily when installing from an ISO, then change it back — otherwise the machine reinstalls, or stalls at the installer, on every reboot.

> **Warning:** A machine with no bootable device enabled will not start. If it fails to boot after a change, check the boot order first.

---

### Screenshot 4

**Boot Order Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Boot Order edit dialog, showing the device list with enable checkboxes
> and ordering controls.

---

## Step 6: Enable Protection

Prevents accidental deletion.

1. Select **Protection**.
2. Click **Edit**.
3. Tick to enable.
4. Click **OK**.

With protection on, the machine and its disks cannot be removed until it is turned off again. Worth enabling on anything you would be sorry to lose.

---

## Step 7: Enable the QEMU Guest Agent

Improves integration between VM2Cloud and the guest operating system.

1. Select **QEMU Guest Agent**.
2. Click **Edit**.
3. Tick to enable.
4. Click **OK**.
5. Install the guest agent package **inside** the guest.
6. Restart the machine.

Both halves are required. Enabling the option without installing the package in the guest achieves nothing.

With the agent working, VM2Cloud can request a clean shutdown from the guest, report the guest's IP addresses on the Summary page, and quiesce the filesystem during snapshot-mode backups — which is what makes those backups consistent for databases.

---

### Screenshot 5

**QEMU Guest Agent Setting**

```text
[ Place Screenshot Here ]
```

> **Capture:** The QEMU Guest Agent edit dialog on the Options tab.

---

# Configuration / Options

| Option | Description | Applies |
|---|---|---|
| **Name** | Display name of the machine. | Immediately |
| **Start at boot** | Start automatically when the node boots. | Next node boot |
| **Start/Shutdown order** | Order, startup delay, and shutdown timeout. | Next node boot or shutdown |
| **OS Type** | Guest operating system type. Tunes virtual hardware defaults. | Next start |
| **Boot Order** | Which devices to boot from, and in what sequence. | Next start |
| **Use tablet for pointer** | Absolute pointing device. Improves mouse behaviour in graphical consoles. | Next start |
| **Hotplug** | Which device types may be added or removed while running. | Next start |
| **ACPI support** | Enables ACPI, required for clean shutdown signalling. | Next start |
| **KVM hardware virtualization** | Hardware acceleration. Disable only for nested or unsupported cases. | Next start |
| **Freeze CPU at startup** | Starts the machine paused, for debugging. | Next start |
| **Use local time for RTC** | Presents local time rather than UTC to the guest. Needed by some guests. | Next start |
| **RTC start date** | Overrides the virtual clock's start date. | Next start |
| **SMBIOS settings** | Identification data presented to the guest. | Next start |
| **QEMU Guest Agent** | Enables agent integration. Requires the package inside the guest. | Next start |
| **Protection** | Blocks deletion of the machine and its disks. | Immediately |
| **Spice Enhancements** | Additional features for SPICE consoles. | Next start |
| **VM State storage** | Where the memory state is written for snapshots that include RAM. | Next snapshot |

> **Verify:** Capture the complete VM Options list and confirm the exact setting names,
> their defaults, and which are present in this deployment. The list above covers the
> settings common to the platform but may not be exhaustive.

---

# Verification

Verify the following:

* The Options list shows the intended values.
* After a node reboot, machines with **Start at boot** enabled came up.
* Machines started in the intended order, with dependencies available first.
* The machine boots from the intended device.
* A protected machine cannot be deleted while protection is on.
* With the guest agent enabled and installed, the Summary page reports the guest's IP addresses.
* A clean shutdown request from the interface stops the guest gracefully.

Test start ordering by rebooting the node during a maintenance window, not by assuming the numbers are right.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Change had no effect | Most options apply at next start. Restart the machine. |
| Machine did not start after node reboot | **Start at boot** is disabled. |
| Machines started in the wrong order | Check the order numbers and startup delays. Lower numbers start first. |
| Application failed after node reboot | Its dependency had not finished starting. Increase the startup delay on the dependency. |
| Machine will not boot | No bootable device is enabled in the boot order, or the intended disk is disabled. |
| Machine keeps booting the installer | The CD/DVD is still first in the boot order. Move the disk above it. |
| Cannot delete a machine | **Protection** is enabled. Disable it first. |
| Guest agent not working | The package is not installed inside the guest, or the machine was not restarted after enabling the option. |
| No IP addresses on the Summary page | The guest agent is not running. Both the option and the in-guest package are required. |
| Shutdown times out and forces off | The guest is not responding to ACPI. Check ACPI support and whether the guest handles the signal. |
| Clock wrong inside the guest | Adjust **Use local time for RTC** to match what the guest expects. |

---

# Best Practices

- Enable **Start at boot** on every production machine, so a node restart does not require manual intervention.
- Set start order deliberately for dependent machines, and give dependencies a startup delay long enough to become ready — not just to start.
- Enable **Protection** on machines that would be painful to lose.
- Install and enable the guest agent on every machine. Better shutdowns, IP reporting, and consistent snapshot backups.
- Return the boot order to disk-first immediately after installing from an ISO.
- Change options during a maintenance window when a restart is required.
- Test start ordering with a real node reboot before relying on it.
- Record why a non-default setting was changed.

---

# Related Documentation

- [Manage Virtual Machine](Manage-Virtual-Machine.md)
- [Manage VM Hardware](Manage-VM-Hardware.md)
- [Create Virtual Machine](Create-Virtual-Machine.md)
- [Cloud-Init](Cloud-Init.md)
- [VM Snapshots](VM-Snapshots.md)
- [Backup and Restore VM](Backup-and-Restore-VM.md)
- [VM Console](VM-Console.md)
- [VM Troubleshooting](VM-Troubleshooting.md)
- [Container Options](../05-Containers/CT-Options.md)

---

# Summary

The Options tab controls how a virtual machine behaves rather than what hardware it has — boot device order, automatic startup with the node, start and shutdown sequencing, deletion protection, and guest agent integration. Most settings apply at the next start rather than immediately.

Three are worth setting on every production machine: **Start at boot**, so a node restart does not leave workloads down; a deliberate **start order** with delays long enough for dependencies to become ready; and the **QEMU Guest Agent**, both enabled here and installed inside the guest, which gives clean shutdowns, IP reporting, and consistent snapshot backups.
