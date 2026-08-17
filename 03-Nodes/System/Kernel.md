# Kernel

---

## Overview

The **Kernel** page provides information about the Linux kernels installed on the selected VM2Cloud VE node. The Linux kernel is the core component of the operating system and is responsible for managing hardware resources, memory, processes, networking, storage, and virtualization.

Administrators can use this page to view the active kernel, review installed kernel versions, select the default boot kernel (when supported), and manage kernel packages.

---

## When to Use

Use the **Kernel** page to:

- View the currently running kernel.
- Review installed kernel versions.
- Select the default kernel for boot (when supported).
- Remove unused kernels.
- Verify the active kernel after a system update.
- Troubleshoot kernel-related issues.

---

## Prerequisites

Before managing kernels, ensure that:

- You are logged in to the VM2Cloud VE web interface.
- You have administrative privileges.
- The selected node is online.

---

# View Installed Kernels

## Step 1: Open the Kernel Page

1. Log in to the VM2Cloud VE web interface.
2. Select the required node.
3. Expand **System**.
4. Select **Kernel**.

---

### Screenshot 1

**Kernel Page**

```text
[ Place Screenshot Here ]
```

> **Capture:** Node → System → Kernel as it opens.

---

## Step 2: Review Installed Kernels

The Kernel page displays information such as:

- Kernel Version
- Running Kernel
- Default Boot Kernel
- Installation Status

Review the information to verify the current kernel configuration.

---

### Screenshot 2

**Installed Kernels**

```text
[ Place Screenshot Here ]
```

> **Capture:** The list showing each version, which is currently running, and which
> boots by default.

---

# Change the Default Kernel

> **Note**
>
> This option is available only when multiple kernels are installed and the VM2Cloud VE version supports selecting the default kernel.

## Step 1: Select the Kernel

1. Select the required kernel.
2. Click **Set as Default** or **Edit** (depending on the VM2Cloud VE version).

---

### Screenshot 3

**Select a Kernel**

```text
[ Place Screenshot Here ]
```

> **Capture:** A kernel selected with the action buttons visible. **This clears a
> `Verify` marker** — whether the control is **Set as Default** or **Edit**.

---

## Step 2: Confirm the Change

1. Review the selected kernel.
2. Confirm the operation.

The selected kernel becomes the default kernel for the next system boot.

---

### Screenshot 4

**Confirm Default Change**

```text
[ Place Screenshot Here ]
```

> **Capture:** The confirmation dialog.

---

# Remove an Unused Kernel

> **Warning:** Never remove the running kernel, and never remove the last remaining
> one — the node will not boot. Keep at least one known-good kernel besides the
> current one, so there is something to fall back to if an update fails.

> **Note**
>
> Remove kernels only if you are certain they are no longer required. Always keep at least one known working kernel installed.

## Step 1: Select the Kernel

1. Select an unused kernel.
2. Click **Remove**.

---

### Screenshot 5

**Remove a Kernel**

```text
[ Place Screenshot Here ]
```

> **Capture:** An unused kernel selected with **Remove** available.

---

## Step 2: Confirm the Removal

1. Review the selected kernel.
2. Confirm the removal.

The selected kernel package is removed from the node.

---

### Screenshot 6

**Removal Confirmation**

```text
[ Place Screenshot Here ]
```

> **Capture:** The confirmation dialog, showing which kernel is about to be removed.

---

## Why Kernel Management Is Important

Proper kernel management helps to:

- Maintain system stability.
- Improve hardware compatibility.
- Receive security updates.
- Support new virtualization features.
- Resolve known kernel issues.
- Ensure compatibility with VM2Cloud VE releases.

---

## Best Practices

- Keep the node updated with supported kernel versions.
- Verify the active kernel after updates.
- Test new kernels before deploying them in production.
- Keep at least one previous working kernel available for recovery.
- Reboot the node after changing the default kernel when required.

---

# Verification

Verify the following:

- The expected kernel is listed.
- The running kernel is correct.
- The default boot kernel is configured as expected.
- The node boots successfully after a kernel change.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Expected kernel is not listed | Verify that the kernel package is installed successfully. |
| System boots into an older kernel | Confirm that the correct default kernel is selected and reboot the node. |
| Unable to remove a kernel | Ensure the selected kernel is not currently running. |
| Node fails to boot after a kernel update | Boot using a previous working kernel and investigate the issue before retrying the update. |

---

# Related Documentation

- System Overview
- Boot Mode
- Update Node
- Node Troubleshooting

---

# Summary

The **Kernel** page allows administrators to view and manage the Linux kernels installed on a VM2Cloud VE node. Proper kernel management helps maintain system stability, improve security, and ensure compatibility with virtualization features and supported hardware.
