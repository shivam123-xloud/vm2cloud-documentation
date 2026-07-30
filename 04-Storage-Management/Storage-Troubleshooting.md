# Storage Troubleshooting

---

## Overview

This guide provides solutions for common storage-related issues that administrators may encounter while configuring or using storage in VM2Cloud. Use this document to identify problems, understand their possible causes, and apply the appropriate resolution.

---

## When to Use

Refer to this guide when:

* A storage resource is unavailable.
* Storage appears offline.
* Virtual machines cannot access storage.
* ISO images or templates cannot be uploaded.
* Storage capacity is exhausted.
* Storage performance is degraded.

---

## Issue 1: Storage is Offline

### Possible Causes

* The storage server is unavailable.
* Network connectivity has been interrupted.
* The storage device is disconnected.
* The storage service has stopped.

### Resolution

* Verify that the storage server is powered on.
* Check network connectivity between the VM2Cloud node and the storage server.
* Confirm that the storage service is running.
* Refresh the Storage page after connectivity has been restored.

---

## Issue 2: Unable to Add Storage

### Possible Causes

* Incorrect configuration.
* Invalid server address.
* Authentication failure.
* Unsupported storage type.

### Resolution

* Verify all configuration details.
* Confirm the server IP address or hostname.
* Check user credentials.
* Ensure the selected storage type is supported.

---

## Issue 3: Storage is Disabled

### Possible Causes

* Storage has been manually disabled.
* Configuration changes were not applied successfully.

### Resolution

* Open **Datacenter → Storage**.
* Select the storage resource.
* Verify that the storage is enabled.
* Save the configuration if any changes are made.

---

## Issue 4: Upload Failed

### Possible Causes

* Insufficient storage space.
* Unsupported file format.
* Network interruption.
* Browser session timeout.

### Resolution

* Verify that sufficient free space is available.
* Confirm that the selected file is supported.
* Retry the upload after restoring network connectivity.
* Refresh the web interface and try again.

---

## Issue 5: Storage Capacity is Full

### Possible Causes

* Virtual machine disks consume all available space.
* Backup files have accumulated.
* Old ISO images or templates are still stored.

### Resolution

* Delete unnecessary ISO images.
* Remove unused backups.
* Delete obsolete templates.
* Expand the storage capacity if required.

---

## Issue 6: Virtual Machine Cannot Use Storage

### Possible Causes

* Storage is disabled.
* The storage does not support the required content type.
* Storage is unavailable.

### Resolution

* Verify that the storage is online.
* Confirm that the required content type is enabled.
* Ensure the storage is assigned to the appropriate node.

---

## Issue 7: Shared Storage is Unreachable

### Possible Causes

* Network connectivity failure.
* Remote storage server is unavailable.
* Incorrect export path or share configuration.

### Resolution

* Verify network connectivity.
* Confirm that the remote storage server is operational.
* Review the export path or shared folder configuration.
* Verify firewall rules if applicable.

---

## Issue 8: Slow Storage Performance

### Possible Causes

* High disk utilization.
* Heavy I/O activity.
* Network congestion.
* Storage hardware limitations.

### Resolution

* Review storage utilization.
* Reduce unnecessary workloads.
* Verify network performance for shared storage.
* Monitor storage hardware health.

---

# Verification

After resolving the issue, verify that:

* The storage status is **Enabled**.
* The storage is accessible.
* Capacity information is displayed correctly.
* Upload operations complete successfully.
* Virtual machines and containers can access the storage without errors.

---

# Summary

Most storage issues are caused by configuration errors, insufficient capacity, connectivity problems, or unavailable storage services. Verifying storage status, available space, network connectivity, and configuration settings will resolve the majority of storage-related problems before advanced troubleshooting is required.
