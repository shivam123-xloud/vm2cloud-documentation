# Shell

---

## Overview

The **Shell** provides administrators with direct command-line access to a VM2Cloud VE node through the web interface. It allows you to perform system administration, execute Linux commands, troubleshoot issues, and manage the node without requiring a separate SSH client.

The Shell opens a terminal session on the selected node and provides the same level of access as a local terminal, subject to the permissions of the logged-in user.

---

## When to Use

Use the Shell to:

- Execute Linux commands.
- Manage system services.
- Troubleshoot node issues.
- Verify network connectivity.
- View system logs.
- Manage storage and files.
- Perform administrative tasks that are not available through the web interface.

---

## Prerequisites

Before using the Shell, ensure that:

- You are logged in to the VM2Cloud VE web interface.
- You have permission to access the selected node.
- The node is online and reachable.

---

# Open the Shell

## Step 1: Select the Node

1. Log in to the VM2Cloud VE web interface.
2. In the navigation tree, select the required node.

---

### Screenshot 1

**Node Selected**

```text
[ Place Screenshot Here ]
```

> **Capture:** The node selected in the tree with the **Shell** button visible.

---

## Step 2: Open the Shell

1. Click **Shell** from the node menu.

A new terminal window opens.

---

### Screenshot 2

**Shell Window**

```text
[ Place Screenshot Here ]
```

> **Capture:** The terminal open at a root prompt.

---

## Step 3: Run Commands

Type the required Linux command and press **Enter**.

Example:

```bash
hostname
```

Example output:

```text
node1
```

---

### Screenshot 3

**Command and Output**

```text
[ Place Screenshot Here ]
```

> **Capture:** A command run in the shell with its output — `hostname` or similar.

---

## Common Administrative Commands

The following commands are commonly used when managing a VM2Cloud VE node.

| Purpose | Command |
|---------|---------|
| Display hostname | `hostname` |
| Display operating system | `cat /etc/os-release` |
| Show disk usage | `df -h` |
| Show memory usage | `free -h` |
| Display CPU information | `lscpu` |
| Show IP addresses | `ip addr` |
| Display routing table | `ip route` |
| Check uptime | `uptime` |
| Display running processes | `ps aux` |
| View system journal | `journalctl` |

---

## Copy and Paste

The web-based Shell supports standard copy and paste operations provided by your operating system and web browser.

When copying command output, ensure that sensitive information such as passwords or private keys is handled securely.

---

## Exit the Shell

To end the terminal session, enter:

```bash
exit
```

Alternatively, close the Shell window.

---

# Verification

Verify the following:

- The Shell opens successfully.
- Commands execute without errors.
- Command output is displayed correctly.
- The terminal responds to keyboard input.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Shell does not open | Verify that the node is online and accessible. |
| Permission denied | Confirm that your account has sufficient privileges. |
| Command not found | Verify that the command is installed and entered correctly. |
| Terminal becomes unresponsive | Refresh the web interface and reopen the Shell. |
| Unable to execute administrative commands | Ensure you are logged in with an account that has the necessary permissions. |

---

# Best Practices

- Verify commands before executing them.
- Use administrative privileges only when necessary.
- Avoid modifying system files unless required.
- Record significant configuration changes.
- Use the Shell for troubleshooting when a graphical option is unavailable.

---

# Summary

The Shell provides secure command-line access to a VM2Cloud VE node directly from the web interface. It enables administrators to perform advanced management and troubleshooting tasks without requiring an external SSH client, making it an essential tool for day-to-day system administration.
