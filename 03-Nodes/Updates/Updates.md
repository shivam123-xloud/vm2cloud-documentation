# Updates

---

## Overview

The **Updates** page allows administrators to view available package updates for the selected VM2Cloud node.

It helps administrators identify available software and security updates before performing system maintenance.

---

## When to Use

Use the **Updates** page to:

- Check for available package updates.
- Review packages that can be updated.
- Verify the current update status of the node.
- Identify packages requiring attention.
- Prepare the node for maintenance.

---

## Prerequisites

Before checking for updates, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have sufficient permissions to view update information.
- The node is online.
- The configured software repositories are accessible.

---

# View Available Updates

## Step 1: Open the Updates Page

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Open **System**.
4. Select **Updates**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Refresh the Package Information

Click **Refresh** to retrieve the latest package information from the configured repositories.

Wait for the update check to complete.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Review Available Updates

Review the packages displayed in the update list.

Information may include:

- Package name
- Current version
- Available version
- Repository
- Package description

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Install Available Updates

If the VM2Cloud interface provides an **Upgrade** or **Update** option:

1. Review the available updates.
2. Confirm that the configured repositories are correct.
3. Start the update operation.
4. Monitor the task output.
5. Wait for the operation to complete.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Reboot After Updates

Some updates, particularly kernel or core system updates, may require a node reboot.

If a reboot is required:

1. Review the update result.
2. Confirm which components require a reboot.
3. Schedule the reboot during an appropriate maintenance window.
4. Reboot the node using the **Reboot Node** procedure.

Do not reboot a production node without considering the impact on running workloads and cluster services.

---

# Verification

After checking or installing updates, verify that:

- The update check completed successfully.
- Available packages are displayed correctly.
- Installed packages report the expected versions.
- No repository errors are reported.
- Required services continue operating normally.
- The node remains accessible.

---

# Common Issues

| Issue | Resolution |
|---|---|
| No updates are displayed | Refresh the package information and verify the configured repositories. |
| Update check fails | Verify network connectivity and repository availability. |
| Repository error is displayed | Review the repository configuration under **Repositories**. |
| Package installation fails | Review the task output and identify the package or dependency causing the failure. |
| Node requires reboot | Schedule a maintenance window and reboot the node after verifying workload impact. |

---

# Best Practices

- Check for updates regularly.
- Review available updates before installing them.
- Keep production nodes on supported VM2Cloud versions.
- Verify repository configuration before performing upgrades.
- Take appropriate backups before major system upgrades.
- Schedule reboots during maintenance windows.
- Test significant updates in a non-production environment when possible.

---

# Related Documentation

- Repositories
- Reboot Node
- Node Troubleshooting
- Certificates

---

# Summary

The **Updates** page allows administrators to check and manage available software updates for a VM2Cloud node. Regularly reviewing updates helps maintain system security, stability, and compatibility while ensuring that the node receives supported software and security fixes.
