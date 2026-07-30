# Manage Storage

---

## Overview

The **Manage Storage** page allows administrators to modify existing storage configurations, enable or disable storage, and remove storage that is no longer required. Proper storage management helps maintain an organized and efficient VM2Cloud environment.

> **Important:** Before modifying or removing storage, ensure that it is not actively being used by virtual machines, containers, backups, or ISO images.

---

## When to Use

Manage storage when you need to:

* Update an existing storage configuration.
* Enable or disable a storage resource.
* Modify the storage content types.
* Change the nodes that can access the storage.
* Remove unused storage from the environment.

---

## Prerequisites

Before managing storage, ensure that:

* You have administrator privileges.
* The storage resource already exists.
* No active tasks are using the storage.
* Important data has been backed up if the storage will be removed.

---

# Edit Storage

## Step 1: Open Storage Management

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Click **Storage**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Select the Storage

1. Select the storage you want to modify.
2. Click **Edit** from the toolbar.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Update the Configuration

Modify the required settings.

Depending on the storage type, you may update:

* Storage ID
* Enabled Content Types
* Node Assignment
* Shared Option
* Storage Path
* Server Information
* Connection Settings

> **Note:** Some settings cannot be modified after the storage has been created.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Save the Changes

1. Review the updated configuration.
2. Click **OK** or **Save**.
3. Wait for the task to complete.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Enable or Disable Storage

## Step 1: Select the Storage

1. Open **Datacenter → Storage**.
2. Select the required storage.
3. Click **Edit**.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Step 2: Change the Status

1. Enable or disable the storage as required.
2. Save the configuration.

> **Note:** Disabling storage prevents new virtual machines, containers, and other resources from using it. Existing data remains on the storage.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Remove Storage

## Step 1: Verify the Storage

Before removing storage, ensure that:

* No virtual machine disks are stored on it.
* No container root filesystems are stored on it.
* No ISO images or templates are required.
* No backup jobs are using the storage.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

## Step 2: Remove the Storage

1. Select the storage.
2. Click **Remove**.
3. Review the confirmation message.
4. Confirm the removal.

---

### Screenshot 8

```text
[ Place Screenshot Here ]
```

---

## Step 3: Verify Removal

1. Refresh the Storage page.
2. Confirm that the storage no longer appears in the list.

---

### Screenshot 9

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* Updated storage settings are displayed correctly.
* Storage is enabled or disabled as expected.
* Removed storage no longer appears in the Storage list.
* No storage-related errors are displayed.

---

# Common Issues

| Issue                                 | Resolution                                                                                             |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Unable to edit storage                | Verify that you have administrator privileges and the storage is available.                            |
| Remove option is unavailable          | Ensure the storage is not currently being used by virtual machines, containers, backups, or templates. |
| Storage remains disabled              | Review the storage configuration and confirm it was enabled successfully.                              |
| Configuration changes are not visible | Refresh the web interface and verify the task completed successfully.                                  |

---

# Summary

The Storage Management page allows administrators to maintain existing storage resources by updating their configuration, controlling their availability, and removing storage that is no longer required. Before making any changes, always verify that the storage is not actively in use to avoid disrupting workloads.
