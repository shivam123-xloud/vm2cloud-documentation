# Update Node

---

## Overview

The **Updates** page allows administrators to view and install available package updates for the selected VM2Cloud VE node.

Keeping a node up to date ensures that it receives the latest bug fixes, security patches, performance improvements, and new features. Updates should be performed regularly as part of routine system maintenance.

---

## When to Use

Update a node when you need to:

* Check for available package updates.
* Install the latest security updates.
* Apply bug fixes.
* Upgrade installed packages.
* Keep all cluster nodes on the same software version.
* Verify the current update status of the node.
* Prepare the environment for new features.

---

## Prerequisites

Before updating a node, ensure that:

* You have administrator privileges.
* The node is online.
* A stable internet connection is available (or a configured local repository).
* The configured software repositories are accessible.
* No critical tasks, backups, or migrations are currently running.
* A recent backup of important virtual machines and containers is available.

---

# Procedure

## Step 1: Open the Node

1. Log in to the VM2Cloud VE web interface.
2. Expand **Datacenter**.
3. Select the node you want to update.

---

### Screenshot 1

**Select Node**

```text
[ Place Screenshot Here ]
```

---

## Step 2: Open Updates

1. From the node navigation menu, click **Updates**.
2. The Updates page displays the current package status.

---

### Screenshot 2

**Updates Page**

```text
[ Place Screenshot Here ]
```

---

## Step 3: Refresh the Package List

1. Click **Refresh**.
2. Wait for VM2Cloud VE to retrieve the latest package information from the configured repositories.

---

### Screenshot 3

**Refresh Updates**

```text
[ Place Screenshot Here ]
```

---

## Step 4: Review Available Updates

1. Review the list of available updates.
2. Check the package names and versions.
3. Confirm that the updates are appropriate for your environment.

Information displayed may include:

* Package name
* Current version
* Available version
* Repository
* Package description

---

### Screenshot 4

**Available Updates**

```text
[ Place Screenshot Here ]
```

---

## Step 5: Install Updates

1. Click **Upgrade**.
2. Review the confirmation message.
3. Click **Upgrade** to begin the installation.
4. Monitor the task output.
5. Wait for the update process to complete.

---

### Screenshot 5

**Upgrade Packages**

```text
[ Place Screenshot Here ]
```

---

## Step 6: Restart the Node (If Required)

Some updates, particularly kernel or core system updates, require a node reboot.

If a reboot is required:

1. Review the update result and confirm which components require a reboot.
2. Schedule the reboot during an appropriate maintenance window.
3. Reboot the node using the [Reboot Node](../Reboot-Node.md) procedure.
4. Wait until the node returns to an online state.

> **Note:** Do not reboot a production node without considering the impact on running workloads and cluster services.

---

# Verification

Verify the following:

* The update check completed successfully.
* The update task completed successfully.
* No failed package installations are reported.
* Installed packages report the expected versions.
* No repository errors are reported.
* The node status is **Online**.
* The Updates page shows no pending updates (or only newly released updates).
* Virtual machines and containers are operating normally after the update.

---

# Common Issues

| Issue                                       | Resolution                                                                              |
| ------------------------------------------- | --------------------------------------------------------------------------------------- |
| No updates are displayed                    | Refresh the package information and verify the configured repositories.                 |
| Unable to refresh package list              | Verify internet connectivity or repository configuration.                               |
| Update process fails                        | Review the task log and resolve the reported package or dependency issue.               |
| Upgrade button is unavailable               | Confirm you have administrator privileges.                                              |
| Repository error is displayed               | Review the repository configuration under [Repositories](Repositories.md).              |
| Node does not come back online after reboot | Verify the server has restarted successfully and check the system console if necessary. |

---

# Best Practices

- Check for updates regularly.
- Review available updates before installing them.
- Keep production nodes on supported VM2Cloud VE versions.
- Verify repository configuration before performing upgrades.
- Take appropriate backups before major system upgrades.
- Schedule reboots during maintenance windows.
- Test significant updates in a non-production environment when possible.
- Keep all cluster nodes on the same software version.

---

# Related Documentation

- [Repositories](Repositories.md)
- [Reboot Node](../Reboot-Node.md)
- [Node Troubleshooting](../Node-Troubleshooting.md)
- [Certificates](../System/Certificates.md)

---

# Summary

Updating a VM2Cloud VE node helps maintain system security, stability, and performance. Regularly reviewing and installing updates ensures that all nodes remain compatible, receive the latest fixes, and continue operating reliably within the VM2Cloud VE environment.
