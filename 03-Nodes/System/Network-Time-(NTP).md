# Network Time (NTP)

---

## Overview

The **Network Time (NTP)** page allows administrators to configure and monitor Network Time Protocol (NTP) synchronization for a VM2Cloud node.

NTP keeps the system clock synchronized with trusted time servers, ensuring accurate timestamps across all nodes. Accurate time synchronization is essential for cluster communication, authentication, logging, scheduled tasks, backups, and High Availability (HA).

---

## When to Use

Use the **Network Time (NTP)** page to:

- Verify that the node is synchronizing its system time.
- Configure NTP servers.
- Review the synchronization status.
- Troubleshoot time synchronization issues.
- Maintain consistent time across cluster nodes.

---

## Prerequisites

Before configuring Network Time, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have administrative privileges.
- The selected node is online.
- The configured NTP servers are reachable.

---

# View NTP Configuration

## Step 1: Open the Network Time Page

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Expand **System**.
4. Select **Network Time (NTP)**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review the Configuration

The page displays the current Network Time configuration.

Typical information includes:

- NTP Service Status
- Configured NTP Servers
- Synchronization Status
- Current Time Source
- Last Synchronization Time (if available)

Review the displayed information to ensure the node is synchronizing correctly.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Configure NTP Servers

## Step 1: Open the Configuration

1. On the **Network Time (NTP)** page, click **Edit**.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the NTP Servers

Enter the required NTP server addresses.

Typical examples include:

- pool.ntp.org
- time.google.com
- time.cloudflare.com
- Your organization's internal NTP server

Click **OK** to save the configuration.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Verify Time Synchronization

After saving the configuration:

- Verify that the NTP service is running.
- Confirm that the node synchronizes with an NTP server.
- Ensure the displayed system time is correct.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Why NTP Is Important

Accurate time synchronization is essential for:

- Cluster communication.
- Corosync operation.
- High Availability (HA).
- Authentication services.
- SSL certificate validation.
- Backup schedules.
- Task scheduling.
- Accurate system logs.

---

## Best Practices

- Configure all cluster nodes to use the same NTP source whenever possible.
- Use reliable and reachable NTP servers.
- Verify synchronization after changing the configuration.
- Monitor the synchronization status regularly.
- Ensure firewall rules allow communication with the configured NTP servers.

---

# Verification

Verify the following:

- The NTP service is running.
- The node is synchronized with a time server.
- The displayed system time is accurate.
- All cluster nodes report consistent system time.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| NTP service is not running | Verify that the Network Time service is enabled and operational. |
| Node is not synchronized | Confirm that the configured NTP servers are reachable and correctly configured. |
| Incorrect system time | Verify the configured time zone and NTP settings. |
| Cluster nodes report different times | Configure all nodes to use the same NTP source and verify synchronization. |

---

# Related Documentation

- System Overview
- Time
- Syslog
- Cluster Overview
- Node Troubleshooting

---

# Summary

The **Network Time (NTP)** page allows administrators to configure and monitor time synchronization for a VM2Cloud node. Maintaining synchronized system time across all nodes improves cluster stability, ensures accurate logging, and supports reliable operation of authentication, scheduling, and High Availability services.
