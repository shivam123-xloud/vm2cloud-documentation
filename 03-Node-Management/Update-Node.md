# Update Node

---

## Overview

Keeping a VM2Cloud node up to date ensures that it receives the latest bug fixes, security patches, performance improvements, and new features. Updates should be performed regularly as part of routine system maintenance.

---

## When to Use

Update a node when you need to:

* Install the latest security updates.
* Apply bug fixes.
* Upgrade installed packages.
* Keep all cluster nodes on the same software version.
* Prepare the environment for new features.

---

## Prerequisites

Before updating a node, ensure that:

* You have administrator privileges.
* The node is online.
* A stable internet connection is available (or a configured local repository).
* No critical tasks, backups, or migrations are currently running.
* A recent backup of important virtual machines and containers is available.

---

# Procedure

## Step 1: Open the Node

1. Log in to the VM2Cloud web interface.
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
2. Wait for VM2Cloud to retrieve the latest package information.

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
4. Wait for the update process to complete.

---

### Screenshot 5

**Upgrade Packages**

```text
[ Place Screenshot Here ]
```

---

## Step 6: Restart the Node (If Required)

1. If prompted, restart the node after the updates have been installed.
2. Wait until the node returns to an online state.

---

### Screenshot 6

**Node Restart**

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The update task completed successfully.
* No failed package installations are reported.
* The node status is **Online**.
* The Updates page shows no pending updates (or only newly released updates).
* Virtual machines and containers are operating normally after the update.

---

# Common Issues

| Issue                                       | Resolution                                                                              |
| ------------------------------------------- | --------------------------------------------------------------------------------------- |
| Unable to refresh package list              | Verify internet connectivity or repository configuration.                               |
| Update process fails                        | Review the task log and resolve the reported package or dependency issue.               |
| Upgrade button is unavailable               | Confirm you have administrator privileges.                                              |
| Node does not come back online after reboot | Verify the server has restarted successfully and check the system console if necessary. |
| Repository error                            | Verify that the configured repositories are accessible and correctly configured.        |

---

# Summary

Updating a VM2Cloud node helps maintain system security, stability, and performance. Regular updates ensure that all nodes remain compatible, receive the latest fixes, and continue operating reliably within the VM2Cloud environment.
