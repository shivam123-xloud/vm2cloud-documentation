# System Troubleshooting

---

## Overview

The **System** section contains essential operating system configuration for a VM2Cloud VE node. Incorrect system settings can affect networking, cluster communication, authentication, logging, package management, and overall system stability.

This guide provides common issues related to the System section and recommended troubleshooting steps.

---

## Verify the Node Status

Before troubleshooting, verify the following:

- The node is powered on.
- The node is reachable on the network.
- The VM2Cloud VE web interface is accessible.
- You are logged in with an account that has administrative privileges.

---

# Common Issues

| Issue | Possible Cause | Resolution |
|--------|----------------|------------|
| Incorrect system time | Time zone or NTP configuration is incorrect | Verify the **Time** and **Network Time (NTP)** configuration. |
| DNS resolution fails | Incorrect DNS server configuration | Verify the configured DNS servers and ensure they are reachable. |
| Hostnames cannot be resolved | Incorrect Hosts entries or DNS configuration | Verify the **Hosts** configuration and DNS settings. |
| System logs are empty | Logging service is unavailable or no recent events exist | Refresh the Syslog page and verify the logging service. |
| NTP is not synchronized | NTP server is unreachable or incorrectly configured | Verify the configured NTP servers and network connectivity. |
| Unexpected boot mode | Server firmware configuration changed | Verify the BIOS/UEFI configuration of the server. |
| Incorrect kernel after reboot | Default kernel not configured correctly | Verify the selected default kernel and reboot the node. |
| Unable to save configuration | Insufficient permissions | Ensure you are logged in as a user with administrative privileges. |

---

# Troubleshooting Checklist

When investigating a system issue, verify the following:

- The node is online.
- The system date and time are correct.
- The correct time zone is configured.
- The node is synchronized with an NTP server.
- DNS servers are configured correctly.
- Host entries are valid.
- System logs do not report critical errors.
- The expected kernel is running.
- The node booted using the expected firmware mode.

---

# Useful CLI Commands

The following commands can help diagnose system-related issues.

### View Current Time

```bash
date
```

---

### Check Time Synchronization

```bash
timedatectl status
```

---

### View DNS Configuration

```bash
cat /etc/resolv.conf
```

---

### View Host Entries

```bash
cat /etc/hosts
```

---

### View System Logs

```bash
journalctl
```

---

### View Running Kernel

```bash
uname -r
```

---

### View Boot Mode

```bash
[ -d /sys/firmware/efi ] && echo "UEFI" || echo "Legacy BIOS"
```

---

### View Hostname

```bash
hostnamectl
```

---

### Check Network Connectivity

```bash
ping 8.8.8.8
```

---

### Test DNS Resolution

```bash
ping google.com
```

---

# Best Practices

- Keep all cluster nodes synchronized using the same NTP source.
- Configure reliable DNS servers.
- Review system logs regularly.
- Keep the node updated with supported kernel versions.
- Verify system settings after updates or maintenance.
- Document configuration changes before applying them.
- Perform changes during scheduled maintenance whenever possible.

---

# Verification

Verify the following after resolving an issue:

- The node is online.
- System time is correct.
- DNS resolution works correctly.
- Hostname resolution is functioning.
- System logs no longer report the issue.
- The expected kernel is running.
- The node operates normally.

---

# Related Documentation

- System Overview
- Time
- DNS
- Hosts
- Syslog
- Network Time (NTP)
- Boot Mode
- Kernel
- Node Troubleshooting

---

# Summary

The **System Troubleshooting** guide helps administrators diagnose and resolve common operating system configuration issues on a VM2Cloud VE node. By systematically verifying time synchronization, DNS, hostname resolution, logging, boot configuration, and kernel information, administrators can quickly restore normal node operation while maintaining a stable and reliable environment.
