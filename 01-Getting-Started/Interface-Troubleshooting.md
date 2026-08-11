# Dashboard Troubleshooting

---

## Overview

This guide provides solutions for common issues that administrators may encounter while using the VM2Cloud Dashboard. The Dashboard is the primary management interface, and any issue affecting it can impact the ability to monitor and manage the virtualization environment.

Use this guide to identify common problems, understand their possible causes, and apply the appropriate resolution.

---

## When to Use

Refer to this guide when:

* The Dashboard does not load.
* Dashboard information is missing or incorrect.
* Resources do not appear in the navigation panel.
* Recent Tasks are not updating.
* Widgets display incorrect information.
* The web interface is slow or unresponsive.

---

# Issue 1: Dashboard Does Not Load

### Possible Causes

* VM2Cloud management services are not running.
* Network connectivity issues.
* Web browser cache.
* Incorrect URL or server address.

### Resolution

* Verify that the VM2Cloud server is reachable.
* Confirm that the correct URL is being used.
* Refresh the browser.
* Clear the browser cache if necessary.
* Verify that the required VM2Cloud services are running.

---

# Issue 2: Navigation Panel is Empty

### Possible Causes

* Insufficient user permissions.
* Resources have not been configured.
* Temporary communication issue.

### Resolution

* Verify your user permissions.
* Refresh the Dashboard.
* Confirm that the resources exist in the environment.

---

# Issue 3: Dashboard Widgets Do Not Display Information

### Possible Causes

* Selected resource is unavailable.
* Communication with the node has failed.
* Resource statistics are temporarily unavailable.

### Resolution

* Verify that the selected resource is online.
* Refresh the Dashboard.
* Confirm that the node is reachable.

---

# Issue 4: Recent Tasks Are Not Updating

### Possible Causes

* No administrative operations have been performed.
* Browser session is outdated.
* Temporary communication issue.

### Resolution

* Refresh the Dashboard.
* Perform a new administrative task and verify that it appears.
* Log out and sign in again if necessary.

---

# Issue 5: Search Does Not Return Results

### Possible Causes

* Incorrect search keyword.
* Resource no longer exists.
* User does not have permission to view the resource.

### Resolution

* Verify the search term.
* Refresh the Dashboard.
* Confirm that the resource exists and that your account has permission to access it.

---

# Issue 6: Dashboard Performance is Slow

### Possible Causes

* High resource utilization.
* Multiple background tasks are running.
* Network latency.
* Browser performance issues.

### Resolution

* Review CPU and memory utilization.
* Wait for active tasks to complete.
* Verify network connectivity.
* Reload the web interface.

---

# Issue 7: Unable to Log In to the Dashboard

### Possible Causes

* Incorrect username or password.
* User account is disabled.
* Authentication service is unavailable.

### Resolution

* Verify your login credentials.
* Confirm that your account is active.
* Contact the system administrator if the issue persists.

---

# Issue 8: Dashboard Displays Incorrect Information

### Possible Causes

* Information has not refreshed.
* Background task is still running.
* Temporary communication issue.

### Resolution

* Refresh the Dashboard.
* Wait for running tasks to complete.
* Verify the status directly from the affected resource.

---

# Common Diagnostic Commands

If the Dashboard cannot be accessed or information is not updating correctly, verify the status of the VM2Cloud management services from the affected node.

---

## Check VM2Cloud Web Service

Run the following command on the affected node:

```bash
systemctl status pveproxy
```

This command verifies whether the VM2Cloud web interface service is running.

---

## Check VM2Cloud API Service

Run the following command on the affected node:

```bash
systemctl status pvedaemon
```

This command checks the status of the VM2Cloud API service responsible for handling management requests.

---

## Check Cluster File System Service

Run the following command on the affected node:

```bash
systemctl status pve-cluster
```

This command verifies that the cluster configuration service is operating correctly.

---

## Check Running Tasks

Run the following command on the affected node:

```bash
ps -ef | grep pve
```

This command lists active VM2Cloud-related processes and can help verify that required services are running.

---

## Verify Network Connectivity

Run the following command on the affected node:

```bash
ip addr
```

This command displays the configured network interfaces and IP addresses.

---

## View Recent System Logs

Run the following command on the affected node:

```bash
journalctl -xe
```

This command displays recent system log entries that may help identify service failures or operating system issues.

---

# Verification

After resolving the issue, verify the following:

* The Dashboard loads successfully.
* Navigation resources are displayed.
* Dashboard widgets display current information.
* Recent Tasks are updating correctly.
* Search returns expected results.
* Administrative operations complete successfully.

---

# Summary

Most Dashboard issues are caused by service availability, network connectivity, user permissions, or temporary communication problems. Verifying the status of the VM2Cloud services, refreshing the Dashboard, and reviewing recent system logs will resolve the majority of Dashboard-related issues.
