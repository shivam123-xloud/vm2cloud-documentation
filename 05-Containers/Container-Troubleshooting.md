# Container Troubleshooting

---

## Overview

This guide provides solutions for common issues that may occur while creating, starting, managing, migrating, backing up, restoring, or deleting containers in VM2Cloud VE. It also includes commonly used CLI commands for diagnosing container-related issues.

---

## When to Use

Use this guide when:

* A container fails to start.
* A container becomes unresponsive.
* The console cannot be opened.
* Container migration fails.
* Backup or restore operations fail.
* Container performance is slow.
* Resource changes are not applied.
* Network connectivity is unavailable.

---

# Issue 1: Container Does Not Start

### Possible Causes

* Insufficient CPU or memory resources.
* Storage is unavailable.
* Invalid container configuration.
* Another task is already running.

### Resolution

* Verify that sufficient resources are available on the node.
* Confirm that the storage is online.
* Review the container configuration.
* Wait for any running tasks to complete before starting the container again.

---

# Issue 2: Container Is Stuck in Starting State

### Possible Causes

* Storage is unavailable.
* Previous task did not complete successfully.
* High resource utilization on the node.

### Resolution

* Review the **Recent Tasks** for failed operations.
* Verify the storage status.
* Check CPU and memory utilization on the node.
* Retry the operation after the current task completes.

---

# Issue 3: Unable to Open the Console

### Possible Causes

* The container is stopped.
* Browser session issue.
* Temporary communication issue with the node.

### Resolution

* Verify that the container is running.
* Refresh the browser and reopen the console.
* Confirm that the node is online.

---

# Issue 4: Container Does Not Shut Down

### Possible Causes

* A process inside the container is not responding.
* The shutdown operation timed out.

### Resolution

* Attempt a normal **Shutdown** first.
* Wait for the shutdown process to complete.
* If required, use **Stop** to force the container to stop.

> **Warning:** Force stopping a container may result in data loss.

---

# Issue 5: Migration Fails

### Possible Causes

* Target node is offline.
* Insufficient resources on the destination node.
* Storage is unavailable.

### Resolution

* Verify that the destination node is online.
* Confirm that sufficient CPU, memory, and storage resources are available.
* Verify that the required storage is accessible.

---

# Issue 6: Backup Fails

### Possible Causes

* Backup storage is unavailable.
* Insufficient free space.
* Another backup task is already running.

### Resolution

* Verify that the backup storage is online.
* Confirm that sufficient free space is available.
* Retry the backup after resolving any reported issues.

---

# Issue 7: Container Performance Is Slow

### Possible Causes

* High CPU utilization.
* Insufficient memory.
* Heavy disk activity.
* Other containers consuming excessive resources.

### Resolution

* Review CPU and memory allocation.
* Check storage performance.
* Increase allocated resources if required.
* Distribute workloads across available nodes.

---

# Common Diagnostic Commands

These commands can be executed from the **Node Shell** or through an SSH session.

---

## List Containers

```bash
pct list
```

Displays all containers on the current node.

---

## View Container Status

```bash
pct status <CT_ID>
```

Example:

```bash
pct status 101
```

Displays the current status of the specified container.

---

## Display Container Configuration

```bash
pct config <CT_ID>
```

Example:

```bash
pct config 101
```

Displays the complete configuration of the container.

---

## Start a Container

```bash
pct start <CT_ID>
```

Example:

```bash
pct start 101
```

Starts the specified container.

---

## Stop a Container

```bash
pct stop <CT_ID>
```

Example:

```bash
pct stop 101
```

Immediately stops the container.

---

## Gracefully Shut Down a Container

```bash
pct shutdown <CT_ID>
```

Example:

```bash
pct shutdown 101
```

Requests a graceful shutdown of the container.

---

## Restart a Container

```bash
pct reboot <CT_ID>
```

Example:

```bash
pct reboot 101
```

Restarts the specified container.

---

## Enter the Container Shell

```bash
pct enter <CT_ID>
```

Example:

```bash
pct enter 101
```

Opens a shell session inside the running container.

---

## View Container Resource Usage

```bash
pct status <CT_ID>
```

Example:

```bash
pct status 101
```

Displays the current state of the container. For detailed CPU and memory usage, use the **Summary** page in the VM2Cloud VE web interface.

---

# Verification

After resolving the issue, verify that:

* The container starts successfully.
* The container status is **Running**.
* The console opens correctly.
* Applications and services inside the container are functioning.
* Network connectivity is working.
* No new errors appear in the **Recent Tasks** panel.

---

# Summary

Most container-related issues are caused by insufficient resources, storage availability, configuration problems, or incomplete management tasks. Reviewing the **Recent Tasks**, verifying node resources, and using the appropriate `pct` diagnostic commands can help administrators quickly identify and resolve common issues.
