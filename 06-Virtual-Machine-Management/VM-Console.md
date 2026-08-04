# VM Console

---

## Overview

The VM Console provides direct access to the virtual machine's display, allowing administrators to interact with the guest operating system as if they were physically connected to it. It is commonly used during operating system installation, troubleshooting, system recovery, and routine administration.

The console remains available even when network connectivity to the guest operating system is unavailable.

---

## When to Use

Use the VM Console to:

* Install an operating system.
* Monitor the boot process.
* Log in to the virtual machine.
* Perform system recovery.
* Troubleshoot network-related issues inside the guest operating system.
* Manage the virtual machine when SSH or RDP is unavailable.

---

## Prerequisites

Before opening the console, ensure that:

* The virtual machine exists.
* The virtual machine is running.
* You have permission to access the virtual machine.

---

# Open the VM Console

## Step 1: Select the Virtual Machine

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Select the virtual machine.

---

### Screenshot 1

```text id="console01"
[ Place Screenshot Here ]
```

---

## Step 2: Open the Console

1. Click **Console** from the virtual machine toolbar.

The virtual machine console opens in a new tab or within the current window, depending on your browser and VM2Cloud configuration.

---

### Screenshot 2

```text id="console02"
[ Place Screenshot Here ]
```

---

# Use the Console

Once the console opens, you can:

* View the virtual machine display.
* Log in to the guest operating system.
* Install an operating system from an ISO image.
* Perform maintenance tasks.
* Monitor the boot sequence.
* Restart or shut down the guest operating system.

---

### Screenshot 3

```text id="console03"
[ Place Screenshot Here ]
```

---

# Send Special Key Combinations

The Console provides options to send special keyboard shortcuts directly to the virtual machine.

Examples include:

* Ctrl + Alt + Delete
* Ctrl + Alt + Backspace

These options are useful when the guest operating system does not respond to keyboard shortcuts from your local computer.

---

### Screenshot 4

```text id="console04"
[ Place Screenshot Here ]
```

---

# Console Toolbar

Depending on your VM2Cloud version, the console toolbar may provide options such as:

* Refresh the console
* Full Screen mode
* Send Key combinations
* Scale Display
* Clipboard options (if supported)

---

### Screenshot 5

```text id="console05"
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The console opens successfully.
* The virtual machine display is visible.
* Keyboard and mouse input work correctly.
* You can log in to the guest operating system.
* The console responds to user input.

---

# Common Issues

| Issue                              | Resolution                                                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Console does not open              | Verify that the virtual machine is running and that your account has permission to access the console.                         |
| Black screen is displayed          | Wait for the guest operating system to complete booting. If the issue persists, verify that the virtual machine is powered on. |
| Keyboard or mouse does not respond | Click inside the console window to capture input and try again.                                                                |
| Console disconnects                | Refresh the browser and reopen the console. Verify that the node is online.                                                    |
| Unable to log in                   | Verify the guest operating system credentials. VM2Cloud does not manage guest operating system user accounts.                  |

---

# Summary

The VM Console provides direct access to a virtual machine without requiring network connectivity to the guest operating system. It is an essential tool for operating system installation, troubleshooting, maintenance, and recovery, allowing administrators to manage virtual machines even when remote access services are unavailable.
