# VM Troubleshooting

---

## Overview

This guide provides solutions for common issues that may occur while creating, starting, managing, or deleting virtual machines in VM2Cloud VE. It also includes basic diagnostic steps to help identify and resolve problems before further investigation.

---

## When to Use

Use this guide when:

* A virtual machine fails to start.
* A virtual machine becomes unresponsive.
* The console cannot be opened.
* Migration fails.
* Backup or restore operations fail.
* Snapshot operations fail.
* Hardware changes are not applied.
* A virtual machine performs poorly.

---

# Issue 1: Virtual Machine Does Not Start

### Possible Causes

* Insufficient CPU or memory resources.
* Storage is unavailable.
* ISO or boot disk is missing.
* Another task is already running.

### Resolution

* Verify that the node has sufficient available resources.
* Confirm that the storage is online.
* Verify that the boot disk is attached.
* Wait for any running tasks to complete before starting the virtual machine again.

---

# Issue 2: Virtual Machine Is Stuck in Starting State

### Possible Causes

* Storage is slow or unavailable.
* A previous task did not complete successfully.
* The node is experiencing high resource utilization.

### Resolution

* Review the Recent Tasks for any failed operations.
* Verify the storage status.
* Check the node's CPU and memory usage.
* Retry the operation after the current task completes.

---

# Issue 3: Unable to Open the Console

### Possible Causes

* The virtual machine is powered off.
* Browser session issue.
* Temporary communication issue with the node.

### Resolution

* Verify that the virtual machine is running.
* Refresh the browser and reopen the console.
* Confirm that the node is online.

---

# Issue 4: Virtual Machine Does Not Shut Down

### Possible Causes

* The guest operating system is not responding.
* ACPI shutdown is not supported.
* QEMU Guest Agent is unavailable.

### Resolution

* Attempt a normal **Shutdown** first.
* Wait for the guest operating system to respond.
* If necessary, use **Stop** to force power off the virtual machine.

> **Warning:** Force stopping a virtual machine may result in data loss.

---

# Issue 5: Migration Fails

### Possible Causes

* Target node is offline.
* Insufficient resources on the target node.
* Storage configuration does not support the selected migration.

### Resolution

* Verify that the destination node is online.
* Confirm that sufficient CPU, memory, and storage resources are available.
* Review the migration configuration before retrying.

---

# Issue 6: Snapshot Creation Fails

### Possible Causes

* Storage does not support snapshots.
* Insufficient free storage space.
* Another snapshot operation is already running.

### Resolution

* Verify that the storage supports snapshots.
* Check available storage capacity.
* Wait for active tasks to complete before retrying.

---

# Issue 7: Backup Fails

### Possible Causes

* Backup storage is unavailable.
* Insufficient free storage space.
* Another backup task is already running.

### Resolution

* Verify that the backup storage is online.
* Confirm that sufficient storage space is available.
* Retry the backup after resolving any reported errors.

---

# Issue 8: Virtual Machine Performance Is Slow

### Possible Causes

* High CPU utilization.
* Insufficient memory.
* Slow storage performance.
* Excessive resource usage by other virtual machines.

### Resolution

* Review CPU and memory usage.
* Verify storage performance.
* Consider increasing allocated resources if required.
* Balance workloads across available nodes.

---

# Common Diagnostic Commands

These commands can be run from the **Node Shell** or through an SSH session.

---

## List Virtual Machines

```bash
qm list
```

Displays all virtual machines available on the node.

---

## View Virtual Machine Status

```bash
qm status <VM_ID>
```

Example:

```bash
qm status 101
```

Displays the current state of the specified virtual machine.

---

## Display Virtual Machine Configuration

```bash
qm config <VM_ID>
```

Example:

```bash
qm config 101
```

Displays the complete configuration of the virtual machine.

---

## Start a Virtual Machine

```bash
qm start <VM_ID>
```

Example:

```bash
qm start 101
```

Starts the specified virtual machine.

---

## Stop a Virtual Machine

```bash
qm stop <VM_ID>
```

Example:

```bash
qm stop 101
```

Immediately stops the virtual machine.

---

## Gracefully Shut Down a Virtual Machine

```bash
qm shutdown <VM_ID>
```

Example:

```bash
qm shutdown 101
```

Requests a graceful shutdown of the guest operating system.

---

## Restart a Virtual Machine

```bash
qm reboot <VM_ID>
```

Example:

```bash
qm reboot 101
```

Restarts the virtual machine.

---

## Monitor Running Tasks

Review the **Recent Tasks** section in the VM2Cloud VE interface to identify failed or incomplete operations and use the task details to determine the cause of the issue.

---

# Verification

After resolving the issue, verify that:

* The virtual machine starts successfully.
* The VM status is **Running**.
* The console opens correctly.
* Applications are accessible.
* Network connectivity is functioning.
* No new errors appear in the Recent Tasks.

---

# Summary

Most virtual machine issues are related to insufficient resources, storage availability, network configuration, or incomplete management tasks. By reviewing the Recent Tasks, verifying resource availability, and using the appropriate diagnostic commands, administrators can resolve the majority of common virtual machine problems quickly and efficiently.
