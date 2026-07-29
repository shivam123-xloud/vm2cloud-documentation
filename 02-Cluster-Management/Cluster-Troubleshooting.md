# Cluster Troubleshooting

---

## Overview

This guide provides solutions for common issues that may occur while creating, joining, or managing a VM2Cloud cluster. Use this document to identify problems, understand their possible causes, and apply the appropriate resolution.

---

## When to Use

Refer to this guide when:

* Cluster creation fails.
* A node cannot join the cluster.
* A node appears offline.
* Cluster communication is interrupted.
* Cluster services fail to start.
* The cluster status shows warnings or errors.

---

## Prerequisites

Before troubleshooting, ensure that:

* You have administrator privileges.
* You have access to the affected node.
* The management network is reachable.
* The affected node is powered on.

---

# Common Issues

---

## Issue 1: Unable to Create a Cluster

### Possible Causes

* Hostname is not configured correctly.
* Network configuration is incorrect.
* DNS or hostname resolution is failing.
* Cluster services are already configured.

### Resolution

* Verify the server hostname.
* Verify the management IP address.
* Check network connectivity.
* Ensure the node is not already part of another cluster.
* Retry the cluster creation process.

---

## Issue 2: Unable to Join a Cluster

### Possible Causes

* Incorrect Join Information.
* Incorrect administrator password.
* Network communication failure.
* Version mismatch between nodes.

### Resolution

* Copy the Join Information again.
* Verify the administrator credentials.
* Confirm that all nodes can communicate over the management network.
* Ensure all nodes are running the same VM2Cloud version.

---

## Issue 3: Node Appears Offline

### Possible Causes

* The node is powered off.
* Network connectivity is unavailable.
* Cluster services are not running.

### Resolution

* Verify the node is powered on.
* Check network connectivity.
* Restart the required cluster services if necessary.
* Confirm the node is reachable from the other cluster members.

---

## Issue 4: Cluster Communication Error

### Possible Causes

* Network interruption.
* Incorrect network configuration.
* Firewall blocking cluster communication.

### Resolution

* Verify the cluster network configuration.
* Confirm all required ports are accessible.
* Review firewall rules.
* Restore network connectivity.

---

## Issue 5: Cluster Page Does Not Update

### Possible Causes

* Browser cache.
* Temporary communication issue.
* Background task still running.

### Resolution

* Refresh the web interface.
* Wait for the running task to complete.
* Log out and log back in.
* Verify that the cluster is healthy.

---

## Issue 6: Cluster Services Fail to Start

### Possible Causes

* Configuration error.
* Corrupted cluster configuration.
* Previous cluster configuration still exists.

### Resolution

* Review the service status.
* Verify the cluster configuration.
* Correct any configuration issues.
* Restart the affected services.

---

## Issue 7: Node Cannot Be Removed

### Possible Causes

* Virtual machines are still running.
* Containers still exist on the node.
* Cluster tasks are still active.

### Resolution

* Migrate all workloads.
* Stop active tasks.
* Retry the node removal process.

---

## Issue 8: Cluster Cannot Be Deleted

### Possible Causes

* Additional nodes still exist.
* Cluster services are still active.
* Cluster configuration has not been removed completely.

### Resolution

* Remove all additional nodes first.
* Stop cluster services.
* Remove the cluster configuration.
* Verify the node is operating as a standalone server.

---

# Verification

After resolving the issue, verify that:

* All cluster nodes are online.
* No warning or error icons are displayed.
* The Cluster page loads successfully.
* Cluster operations complete without errors.
* Recent Tasks shows successful completion.

---

# Summary

Most cluster-related issues are caused by incorrect network configuration, hostname resolution, service configuration, or cluster membership. Verifying these areas first can resolve the majority of problems without requiring additional recovery steps. If the issue persists, review the system logs and service status before making further changes.
