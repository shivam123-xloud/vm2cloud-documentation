# Upload Content

---

## Overview

The **Upload Content** feature allows administrators to upload files that can be used by VM2Cloud. Depending on the storage configuration, supported content includes ISO images, container templates, backup files, and snippets.

Before uploading any content, verify that the selected storage supports the required content type.

---

## When to Use

Upload content when you need to:

* Upload an operating system ISO image.
* Upload a Linux container template.
* Import a backup file.
* Upload snippets for Cloud-Init or automation (if supported).
* Make installation media available for virtual machines.

---

## Prerequisites

Before uploading content, ensure that:

* You have administrator privileges.
* The storage is online and accessible.
* The storage supports the content type being uploaded.
* Sufficient free space is available.

---

# Upload ISO Images

## Step 1: Open the Storage

1. Log in to the VM2Cloud web interface.
2. Expand the required node.
3. Select the storage where the ISO will be uploaded (for example, **local**).
4. Click **ISO Images** or **Content**, depending on your VM2Cloud version.

---

### Screenshot 1

```text id="k4ns2u"
[ Place Screenshot Here ]
```

---

## Step 2: Upload the ISO

1. Click **Upload**.
2. Click **Select File**.
3. Browse and select the ISO image.
4. Click **Upload**.

---

### Screenshot 2

```text id="e0f7da"
[ Place Screenshot Here ]
```

---

## Step 3: Verify the Upload

After the upload completes:

* The ISO appears in the storage content list.
* The upload task completes successfully.

---

### Screenshot 3

```text id="lh9mv3"
[ Place Screenshot Here ]
```

---

# Upload Container Templates

## Step 1: Open Container Templates

1. Select the required storage.
2. Open **CT Templates**.

---

### Screenshot 4

```text id="8onvyd"
[ Place Screenshot Here ]
```

---

## Step 2: Upload or Download a Template

Depending on your VM2Cloud configuration, you can:

* Upload a local container template.
* Download a template from the configured template repository.

Select the required template and complete the operation.

---

### Screenshot 5

```text id="v8m4ak"
[ Place Screenshot Here ]
```

---

## Step 3: Verify the Template

Verify that the container template appears in the **CT Templates** list.

---

### Screenshot 6

```text id="vtkzbw"
[ Place Screenshot Here ]
```

---

# Upload Backup Files

## Step 1: Open Backup Storage

1. Select the storage configured for backups.
2. Open the **Backups** or **Content** tab.

---

### Screenshot 7

```text id="2ewtv2"
[ Place Screenshot Here ]
```

---

## Step 2: Upload the Backup

1. Click **Upload**.
2. Select the backup file.
3. Click **Upload**.
4. Wait for the upload to complete.

---

### Screenshot 8

```text id="8p0e7m"
[ Place Screenshot Here ]
```

---

## Step 3: Verify the Backup

Confirm that the backup file appears in the storage content list.

---

### Screenshot 9

```text id="lmfdxs"
[ Place Screenshot Here ]
```

---

# Upload Snippets (If Supported)

If the selected storage supports **Snippets**, upload the required configuration or automation files using the same upload process.

---

### Screenshot 10

```text id="t17wgu"
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The uploaded file appears in the appropriate content list.
* The upload task completed successfully.
* The file is available for use.
* No upload errors are displayed.

---

# Common Issues

| Issue                             | Resolution                                                            |
| --------------------------------- | --------------------------------------------------------------------- |
| Upload option is unavailable      | Verify that the selected storage supports the content type.           |
| Upload fails                      | Confirm sufficient free space is available on the storage.            |
| File does not appear after upload | Refresh the page and verify the upload task completed successfully.   |
| Unsupported file format           | Ensure the selected file is compatible with the chosen content type.  |
| Permission denied                 | Verify that your account has the required administrative permissions. |

---

# Summary

The Upload Content feature allows administrators to add ISO images, container templates, backup files, and other supported content to VM2Cloud storage. Once uploaded, these files become available for virtual machine deployment, container creation, backup restoration, and other administrative tasks.
