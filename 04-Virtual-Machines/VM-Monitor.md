# VM Monitor

---

## Overview

The **Monitor** tab provides a direct command interface to the virtualization layer running a virtual machine — the hypervisor process itself, not the guest operating system.

This is a low-level diagnostic tool. It talks to the emulator managing the machine's virtual hardware, so it can report things no other tab can: what devices are actually attached, what the emulator believes the machine's state is, and details of the virtual hardware as the hypervisor sees it.

It is not a shell inside the guest. For that, use the [Console](VM-Console.md).

> **Warning:** Monitor commands act directly on the running machine at the hypervisor level, bypassing the safeguards the rest of the interface provides. Some commands can pause, reset, or destabilise a machine, or interrupt in-flight disk writes. Use read-only inspection commands unless you know exactly what a command does.

---

## When to Use

Use the Monitor tab when you need to:

* Inspect virtual hardware as the hypervisor sees it, rather than as configured.
* Investigate a machine behaving oddly in ways the guest cannot explain.
* Confirm which devices are actually attached to a running machine.
* Gather diagnostic detail when escalating a problem.
* Check emulator state during a hang.

Do **not** use it for:

* Anything achievable through the normal tabs — those exist for a reason.
* Routine administration.
* Changing hardware. Use [Manage VM Hardware](Manage-VM-Hardware.md).
* Access to the guest operating system. Use the [Console](VM-Console.md).

Most administrators never need this tab. Reach for it when a problem cannot be explained from the guest or from the machine's configuration.

---

## Prerequisites

* You have administrator privileges. Monitor access is effectively low-level control of the machine.
* The virtual machine is **running**. The monitor is the interface to a live emulator process, so it is unavailable when the machine is stopped.
* You understand what a command does before running it.

---

# Procedure

## Step 1: Open the Monitor Tab

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select a **running** virtual machine.
4. Click **Monitor**.

A command prompt is presented.

---

### Screenshot 1

**VM Monitor Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A running virtual machine → Monitor, showing the command prompt and any
> banner text presented on opening.

---

## Step 2: List the Available Commands

Start here rather than guessing.

```text
help
```

This lists the commands the emulator supports, which depends on the platform version and the machine's configuration.

---

### Screenshot 2

**Command List**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Monitor tab after running `help`, showing the returned command list.

---

## Step 3: Run Inspection Commands

Read-only commands are safe and are what this tab is genuinely useful for.

| Command | Reports |
|---|---|
| `info status` | The emulator's view of whether the machine is running or paused. |
| `info block` | Block devices attached, and the backing files behind them. |
| `info network` | Network devices and how they are connected. |
| `info cpus` | Virtual CPUs and their state. |
| `info pci` | Devices on the virtual PCI bus. |
| `info usb` | Attached USB devices. |
| `info memory-devices` | Memory devices and their sizes. |

`info block` is often the most revealing. It shows what the hypervisor has actually opened, which occasionally differs from what the configuration says — a stale ISO still attached, or a disk pointing at an unexpected path.

> **Verify:** Capture the output of `info status` and `info block` on a running machine
> in this deployment, and confirm which `info` subcommands are available.

---

### Screenshot 3

**Inspection Command Output**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Monitor tab showing the output of `info block` on a running machine.

---

## Step 4: Compare Against the Configuration

The value of this tab comes from comparison.

1. Note what Monitor reports.
2. Open [Manage VM Hardware](Manage-VM-Hardware.md).
3. Compare.

A difference between the two is meaningful. Configuration describes what the machine *should* have; Monitor describes what it *does* have. Hardware added while the machine was running, or a change that failed to apply, shows up as exactly this kind of mismatch.

---

# Configuration / Options

The Monitor tab is a command interface, not a settings panel.

| Command type | Safety |
|---|---|
| `help` | Safe. Lists available commands. |
| `info <subcommand>` | Safe. Read-only inspection. |
| State-changing commands | **Not safe.** Can pause, reset, or destabilise the machine. |

> **Warning:** Commands that change machine state bypass the interface's safeguards and are not logged as normal tasks. A machine stopped this way may leave no entry in [Task History](VM-Task-History.md), making the change hard to account for later. Use the normal tabs for anything that changes state.

---

# Verification

Verify the following:

* The Monitor tab opens and accepts commands on a running machine.
* `help` returns a command list.
* `info status` reports the expected state.
* Devices reported match the configured hardware.
* The machine continues running normally after inspection.
* Any discrepancy found is investigated rather than ignored.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Monitor tab unavailable or empty | The machine is stopped. The monitor exists only while the emulator is running. |
| Command not recognised | Run `help`. The available set depends on the platform version and machine configuration. |
| Expecting a guest shell | Wrong tab. Monitor talks to the hypervisor; use the [Console](VM-Console.md) for the guest. |
| Machine paused unexpectedly | A state-changing command was run. Resume it from [Manage Virtual Machine](Manage-Virtual-Machine.md). |
| Reported devices differ from configuration | Genuine finding. Hardware may have been changed while running, or a change failed to apply. |
| No output from a command | The command may need arguments. Check `help`. |
| Cannot open the tab | Confirm you have sufficient privileges. |

---

# Best Practices

- Treat this as a **diagnostic** tool. If another tab can do the job, use that instead.
- Stay with `info` commands unless you are certain what a command does.
- Run `help` before assuming a command exists.
- Record Monitor output when escalating a problem — it is exactly the detail that gets asked for.
- Compare against the configured hardware rather than reading the output in isolation.
- Never use state-changing commands on production machines as a shortcut; those actions bypass logging.
- Restrict Monitor access to administrators, since it is effectively low-level control of the machine.

---

# Related Documentation

- [VM Console](VM-Console.md)
- [Manage VM Hardware](Manage-VM-Hardware.md)
- [Manage Virtual Machine](Manage-Virtual-Machine.md)
- [VM Summary](VM-Summary.md)
- [VM Options](VM-Options.md)
- [VM Task History](VM-Task-History.md)
- [VM Troubleshooting](VM-Troubleshooting.md)

---

# Summary

The Monitor tab is a direct command interface to the hypervisor process running a virtual machine — useful for seeing what virtual hardware is actually attached and what state the emulator believes the machine is in, which occasionally differs from the configuration.

It is a diagnostic tool, not an administration one. `help` and the `info` commands are safe and cover nearly every legitimate use. Commands that change state bypass the interface's safeguards and its task logging, so use the normal tabs for anything that alters the machine. And it is not a guest shell — that is the Console.
