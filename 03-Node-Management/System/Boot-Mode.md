# Boot Mode

---

## Overview

The **Boot Mode** page displays how the selected VM2Cloud node was started. It indicates whether the node is running in **UEFI** or **Legacy BIOS** mode.

Knowing the boot mode is important when troubleshooting boot-related issues, installing operating systems, configuring boot loaders, or planning hardware upgrades.

The Boot Mode page is informational and does not modify the current boot configuration.

---

## When to Use

Use the **Boot Mode** page to:

- Verify the node's boot mode.
- Confirm whether the system is using UEFI or Legacy BIOS.
- Troubleshoot boot issues.
- Verify firmware configuration after installation.
- Confirm system compatibility before performing upgrades.

---

## Prerequisites

Before viewing the Boot Mode information, ensure that:

- You are logged in to the VM2Cloud web interface.
- The selected node is online.

---

# View Boot Mode

## Step 1: Open the Boot Mode Page

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Expand **System**.
4. Select **Boot Mode**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review the Boot Information

The Boot Mode page displays the current firmware mode used by the node.

Typical information includes:

- Boot Mode
- Firmware Type

Review the displayed information to verify the system configuration.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Boot Modes

### UEFI

UEFI (Unified Extensible Firmware Interface) is the modern firmware standard used by most current servers.

Benefits include:

- Faster startup
- Support for GPT partition tables
- Larger disk support
- Improved security features
- Secure Boot support (when enabled)

---

### Legacy BIOS

Legacy BIOS is the traditional firmware interface used by older systems.

Although fully supported, it has limitations compared to UEFI, including support for smaller boot disks and older partitioning methods.

---

## Best Practices

- Use UEFI for new VM2Cloud installations whenever possible.
- Maintain the same boot mode across similar hardware deployments.
- Verify the firmware mode before reinstalling the operating system.
- Avoid changing the firmware mode after installation unless required.

---

# Verification

Verify the following:

- The displayed boot mode matches the server firmware configuration.
- The node boots successfully.
- No firmware-related errors are reported.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Unexpected boot mode | Verify the firmware settings in the server BIOS/UEFI configuration. |
| System fails to boot after firmware changes | Restore the previous firmware mode or reinstall the boot loader if required. |
| Boot mode does not match deployment requirements | Review the server firmware configuration before reinstalling the operating system. |

---

# Related Documentation

- System Overview
- Kernel
- Node Troubleshooting

---

# Summary

The **Boot Mode** page provides information about how the VM2Cloud node was started. Verifying the firmware mode helps administrators troubleshoot boot issues, validate system configuration, and ensure compatibility with deployment requirements.
