# Time and Network Time (NTP)

---

## Overview

The **Time** page displays the current system date, time, and time zone configured on the selected VM2Cloud VE node, and allows administrators to configure Network Time Protocol (NTP) synchronization.

NTP keeps the system clock synchronized with trusted time servers, ensuring accurate timestamps across all nodes. Accurate system time is essential for cluster communication, authentication, logging, backups, scheduled tasks, and High Availability (HA).

---

## When to Use

Use the **Time** page to:

- Verify the system date and time.
- View the configured time zone.
- Configure the node's time zone.
- Verify that the node is synchronizing its system time.
- Configure NTP servers.
- Review the synchronization status.
- Maintain consistent time across cluster nodes.
- Troubleshoot time-related and synchronization issues.

---

## Prerequisites

Before modifying the time settings, ensure that:

- You are logged in to the VM2Cloud VE web interface.
- You have administrative privileges.
- The selected node is online.
- The configured NTP servers are reachable.

---

# View the System Time

## Step 1: Open the Time Page

1. Log in to the VM2Cloud VE web interface.
2. Select the required node.
3. Expand **System**.
4. Select **Time**.

---

### Screenshot 1

**Time Page**

```text
[ Place Screenshot Here ]
```

> **Capture:** Node → System → Time as it opens.

---

## Step 2: Review the Time Information

The **Time** page displays information such as:

- Current Date
- Current Time
- Configured Time Zone

Review the displayed information to verify that the node is using the correct system time.

---

### Screenshot 2

**Time Information**

```text
[ Place Screenshot Here ]
```

> **Capture:** The same page showing current date, current time, and configured time
> zone.

---

# Change the Time Zone

## Step 1: Open the Time Zone Configuration

1. On the **Time** page, click **Time Zone**.

---

### Screenshot 3

**Time Zone Control**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Time page with the **Time Zone** control visible.

---

## Step 2: Select the Time Zone

1. Choose the appropriate time zone.
2. Click **OK**.

The selected time zone is applied to the node.

---

### Screenshot 4

**Time Zone Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The time-zone selector open, listing the available zones.

---

# View NTP Configuration

## Step 1: Open the Network Time Configuration

1. On the **Time** page, review the Network Time section.

The page displays the current Network Time configuration.

Typical information includes:

- NTP Service Status
- Configured NTP Servers
- Synchronization Status
- Current Time Source
- Last Synchronization Time (if available)

---

### Screenshot 5

**Network Time Section**

```text
[ Place Screenshot Here ]
```

> **Capture:** The NTP portion of the Time page — service status and configured servers.

---

## Step 2: Review the Configuration

Review the displayed information to ensure the node is synchronizing correctly.

---

### Screenshot 6

**Synchronization Status**

```text
[ Place Screenshot Here ]
```

> **Capture:** The same section showing the current time source and last
> synchronization.

---

# Configure NTP Servers

## Step 1: Open the Configuration

1. On the **Time** page, click **Edit** in the Network Time section.

---

### Screenshot 7

**Edit Network Time**

```text
[ Place Screenshot Here ]
```

> **Capture:** The **Edit** control for the Network Time section.

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

### Screenshot 8

**NTP Server Configuration**

```text
[ Place Screenshot Here ]
```

> **Capture:** The NTP edit dialog with servers entered, showing exactly how multiple
> servers are separated.

---

## Step 3: Verify Time Synchronization

After saving the configuration:

- Verify that the NTP service is running.
- Confirm that the node synchronizes with an NTP server.
- Ensure the displayed system time is correct.

---

### Screenshot 9

**Verified Synchronization**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Time page after saving, showing the node synchronized and the time
> correct.

---

## Why Accurate Time Is Important

Maintaining the correct system time and synchronization helps ensure:

- Accurate log timestamps.
- Reliable cluster communication.
- Correct Corosync operation.
- Stable High Availability (HA) operations.
- Successful authentication.
- Reliable SSL certificate validation.
- Consistent backup schedules.
- Proper execution of scheduled tasks.

---

## Best Practices

- Configure all cluster nodes to use the correct time zone.
- Configure all cluster nodes to use the same NTP source whenever possible.
- Use reliable and reachable NTP servers.
- Verify the system time after changing the time zone or NTP configuration.
- Monitor the synchronization status regularly.
- Ensure firewall rules allow communication with the configured NTP servers.
- Regularly confirm that the node time remains synchronized with other cluster members.

---

# Verification

Verify the following:

- The displayed date and time are correct.
- The configured time zone matches your deployment location.
- The NTP service is running.
- The node is synchronized with a time server.
- Time changes are reflected correctly.
- All cluster nodes report consistent system time.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Incorrect system time | Verify the configured time zone and NTP settings. |
| Incorrect time zone | Select the appropriate time zone and apply the changes. |
| NTP service is not running | Verify that the Network Time service is enabled and operational. |
| Node is not synchronized | Confirm that the configured NTP servers are reachable and correctly configured. |
| Time differs between cluster nodes | Configure all nodes to use the same NTP source and verify synchronization. |
| Scheduled tasks execute at the wrong time | Confirm the configured time zone and system time. |

---

# Related Documentation

- [System Overview](System-Overview.md)
- [Syslog](Syslog.md)
- [Cluster Overview](../../02-Datacenter/Cluster/Cluster-Overview.md)
- [Node Troubleshooting](../Node-Troubleshooting.md)

---

# Summary

The **Time** page allows administrators to review and configure the system date, time, time zone, and NTP synchronization for a VM2Cloud VE node. Maintaining accurate, synchronized time across all nodes improves cluster stability, ensures accurate logging, and supports reliable operation of authentication, scheduling, backup, and High Availability services.
