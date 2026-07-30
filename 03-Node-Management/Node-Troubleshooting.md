# Node Troubleshooting

---

## Overview

This guide provides solutions for common issues that administrators may encounter while managing VM2Cloud nodes. Use this document to identify common problems, understand their possible causes, and apply the appropriate resolution.

---

## When to Use

Refer to this guide when:

* A node is offline.
* A node is unreachable from the VM2Cloud web interface.
* High CPU or memory usage is affecting performance.
* System updates fail.
* The node cannot be rebooted or shut down.
* Services on the node are not responding.
* Hardware information is missing or incorrect.

---

# Issue 1: Node Appears Offline

### Possible Causes

* The physical server is powered off.
* Network connectivity is unavailable.
* The management interface is down.
* Cluster communication has failed.

### Resolution

* Verify that the server is powered on.
* Check network connectivity.
* Confirm that the management interface is reachable.
* Verify the node status from another cluster node.

---

# Issue 2: Unable to Access the Node

### Possible Causes

* Incorrect user permissions.
* Network connectivity issue.
* Firewall restrictions.
* Management service is unavailable.

### Resolution

* Verify your user account permissions.
* Confirm network connectivity to the node.
* Review firewall settings.
* Verify that the VM2Cloud management services are running.

---

# Issue 3: High CPU Utilization

### Possible Causes

* Multiple virtual machines are consuming CPU resources.
* Background tasks are running.
* Resource overcommitment.

### Resolution

* Review CPU usage from the **Summary** page.
* Identify virtual machines with high CPU usage.
* Stop unnecessary workloads if possible.
* Monitor CPU utilization over time.

---

# Issue 4: High Memory Utilization

### Possible Causes

* Memory-intensive virtual machines.
* Large number of running containers.
* Memory overcommitment.

### Resolution

* Review memory usage from the **Summary** page.
* Identify workloads consuming excessive memory.
* Shut down unused virtual machines or containers.
* Increase physical memory if required.

---

# Issue 5: Node Updates Fail

### Possible Causes

* Repository configuration issues.
* Internet connectivity problems.
* Package conflicts.
* Insufficient disk space.

### Resolution

* Verify repository configuration.
* Check internet connectivity.
* Ensure sufficient free disk space is available.
* Retry the update after resolving the issue.

---

# Issue 6: Reboot or Shutdown Does Not Complete

### Possible Causes

* Running tasks are preventing shutdown.
* Active virtual machines are still operating.
* System services are busy.

### Resolution

* Wait for active tasks to finish.
* Gracefully shut down running virtual machines and containers.
* Retry the operation after confirming no critical tasks are running.

---

# Issue 7: Node Summary Does Not Load

### Possible Causes

* Temporary communication issue.
* Browser cache.
* Management services are unavailable.

### Resolution

* Refresh the web interface.
* Log out and log back in.
* Verify that the node is online.
* Confirm that management services are operational.

---

# Issue 8: Incorrect Hardware Information

### Possible Causes

* Hardware changes have not been detected.
* Temporary communication issue.
* System information has not refreshed.

### Resolution

* Refresh the Summary page.
* Reboot the node if hardware changes were recently made.
* Verify the hardware configuration from the operating system.

---

# Verification

After resolving the issue, verify the following:

* The node status is **Online**.
* The Summary page loads successfully.
* CPU and memory information are displayed correctly.
* Administrative operations complete without errors.
* Virtual machines and containers are operating normally.

---

# Common Diagnostic Commands

The following commands can help verify the health of a VM2Cloud node.

### Check System Uptime

Run the following command on the affected node.

```bash
uptime
```

This command displays the current system uptime and load average.

---

### Check Disk Usage

```bash
df -h
```

This command displays filesystem usage and available disk space.

---

### Check Memory Usage

```bash
free -h
```

This command displays the current memory and swap utilization.

---

### Check Running Processes

```bash
top
```

This command displays the processes currently consuming CPU and memory resources.

---

### Check Network Configuration

```bash
ip addr
```

This command displays the configured network interfaces and IP addresses.

---

### Check Recent System Logs

```bash
journalctl -xe
```

This command displays recent system log entries that may help identify hardware, service, or operating system issues.

---

# Summary

Most node-related issues are caused by hardware availability, network connectivity, resource utilization, or system configuration problems. Reviewing the node status, monitoring system resources, and checking system logs will resolve most issues before advanced troubleshooting is required.
