# Time

---

## Overview

The **Time** page displays the current system date, time, and time zone configured on the selected VM2Cloud node. Accurate system time is essential for cluster communication, authentication, logging, backups, scheduled tasks, and High Availability (HA).

Administrators can review the current time settings and configure the node's time zone when required.

---

## When to Use

Use the **Time** page to:

- Verify the system date and time.
- View the configured time zone.
- Configure the node's time zone.
- Confirm that the node is using the correct local time.
- Troubleshoot time-related issues.

---

## Prerequisites

Before modifying the time settings, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have administrative privileges.
- The selected node is online.

---

# View the System Time

## Step 1: Open the Time Page

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Expand **System**.
4. Select **Time**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review the Time Information

The **Time** page displays information such as:

- Current Date
- Current Time
- Configured Time Zone

Review the displayed information to verify that the node is using the correct system time.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Change the Time Zone

## Step 1: Open the Time Zone Configuration

1. On the **Time** page, click **Time Zone**.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Select the Time Zone

1. Choose the appropriate time zone.
2. Click **OK**.

The selected time zone is applied to the node.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Why Accurate Time Is Important

Maintaining the correct system time helps ensure:

- Accurate log timestamps.
- Reliable cluster communication.
- Successful authentication.
- Proper execution of scheduled tasks.
- Consistent backup schedules.
- Stable High Availability (HA) operations.
- Reliable certificate validation.

---

## Best Practices

- Configure all cluster nodes to use the correct time zone.
- Use Network Time Protocol (NTP) to synchronize the system clock.
- Verify the system time after changing the time zone.
- Regularly confirm that the node time remains synchronized with other cluster members.

---

# Verification

Verify the following:

- The displayed date and time are correct.
- The configured time zone matches your deployment location.
- Time changes are reflected correctly.
- The node remains synchronized with the rest of the cluster.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Incorrect system time | Verify that Network Time (NTP) is configured correctly. |
| Incorrect time zone | Select the appropriate time zone and apply the changes. |
| Time differs between cluster nodes | Verify that all nodes are synchronized using the same NTP source. |
| Scheduled tasks execute at the wrong time | Confirm the configured time zone and system time. |

---

# Related Documentation

- System Overview
- Network Time (NTP)
- Syslog
- Cluster Overview

---

# Summary

The **Time** page allows administrators to review and configure the system date, time, and time zone for a VM2Cloud node. Maintaining accurate time across all nodes helps ensure reliable cluster communication, accurate logging, successful authentication, and proper execution of scheduled operations.
