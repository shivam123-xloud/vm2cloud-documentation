# Services Overview

---

## Overview

The **Services** section allows administrators to monitor and manage the system services running on a VM2Cloud node. These services are responsible for core platform functionality, including web access, cluster communication, virtualization, scheduling, networking, and storage.

Most services start automatically during system boot and continue running in the background. If a required service stops or fails, certain VM2Cloud features may become unavailable or behave unexpectedly.

---

## When to Use

Use the **Services** section to:

- View the status of system services.
- Start, stop, or restart services.
- Review service startup behavior.
- Verify that required services are running.
- Troubleshoot service-related issues.

---

## Prerequisites

Before managing services, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have administrative privileges.
- The selected node is online.

---

## Common VM2Cloud Services

Depending on your deployment, the Services page may include services such as:

| Service | Purpose |
|---------|---------|
| pveproxy | Provides the VM2Cloud web interface. |
| pvedaemon | Handles management requests and background operations. |
| pvestatd | Collects and updates node, storage, and virtual machine statistics. |
| pve-cluster | Manages the cluster file system and configuration synchronization. |
| corosync | Provides cluster communication and quorum management. |
| pve-firewall | Applies firewall rules configured in VM2Cloud. |
| ssh | Enables secure remote command-line access. |
| chronyd | Synchronizes the system clock using Network Time Protocol (NTP). |
| networking | Manages network interfaces and bridges. |

> The list of available services may vary depending on the VM2Cloud version and installed components.

---

## Typical Service Operations

Administrators can perform operations such as:

- View service status.
- Start a stopped service.
- Stop a running service.
- Restart a service.
- Reload service configuration (when supported).
- View service details.

---

## Best Practices

- Stop services only when required for maintenance or troubleshooting.
- Restart services after configuration changes when instructed.
- Verify that critical services are running after a system reboot.
- Avoid stopping cluster-related services on production systems unless necessary.
- Record service changes performed during maintenance.

---

## Related Documentation

- Manage Services
- VM2Cloud Services
- Service Troubleshooting
- Node Troubleshooting

---

## Summary

The **Services** section provides centralized management of the operating system services that support VM2Cloud. Monitoring service health and understanding the purpose of each service helps administrators maintain a stable, secure, and reliable virtualization environment.
