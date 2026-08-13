# Services

---

## Overview

The **Services** section allows administrators to monitor and manage the system services running on a VM2Cloud VE node. These services are responsible for core platform functionality, including web access, cluster communication, virtualization, scheduling, networking, and storage.

Most services start automatically during system boot and continue running in the background. If a required service stops or fails, certain VM2Cloud VE features may become unavailable or behave unexpectedly.

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

- You are logged in to the VM2Cloud VE web interface.
- You have administrative privileges.
- The selected node is online.

---

## Common VM2Cloud VE Services

Depending on your deployment, the Services page may include services such as:

| Service | Purpose |
|---------|---------|
| pveproxy | Provides the VM2Cloud VE web interface. |
| pvedaemon | Handles management requests and background operations. |
| pvestatd | Collects and updates node, storage, and virtual machine statistics. |
| pve-cluster | Manages the cluster file system and configuration synchronization. |
| corosync | Provides cluster communication and quorum management. |
| pve-firewall | Applies firewall rules configured in VM2Cloud VE. |
| ssh | Enables secure remote command-line access. |
| chronyd | Synchronizes the system clock using Network Time Protocol (NTP). |
| networking | Manages network interfaces and bridges. |

> The list of available services depends on which components are installed on the node.

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
- VM2Cloud VE Services
- Service Troubleshooting
- Node Troubleshooting

---

## Common Issues

| Issue | Resolution |
|-------|------------|
| A service will not start | Review the service status and the node's system log for the reported error. |
| A service restarts repeatedly | Investigate the underlying configuration or resource problem before restarting it again. |
| The web interface is unreachable | Verify the proxy service is running on the node. |
| Cluster communication fails | Verify the cluster communication service is running on every node. |
| Changes to a service have no effect | Restart the affected service and confirm the configuration file was saved correctly. |

---

## Summary

The **Services** section provides centralized management of the operating system services that support VM2Cloud VE. Monitoring service health and understanding the purpose of each service helps administrators maintain a stable, secure, and reliable virtualization environment.
