# Create Linux Bridge

---

## Overview

A Linux Bridge connects virtual machines and containers to a physical network. During creation, you can assign a physical network interface, configure IP addressing, enable VLAN awareness, and specify whether the bridge should start automatically when the node boots.

A Linux Bridge can be created for management, production, storage, migration, or other network requirements.

---

## When to Use

Create a Linux Bridge when you need to:

- Connect virtual machines to a network.
- Connect containers to a network.
- Configure a new management network.
- Create a dedicated storage or migration network.
- Configure VLAN-aware networking.

---

## Prerequisites

Before creating a Linux Bridge, ensure that:

- A physical network interface is available.
- The required IP configuration is available.
- The physical switch is configured if VLANs will be used.
- You have permission to manage network settings.

> **Warning:** Creating or modifying network interfaces may temporarily interrupt network connectivity.

---

# Create a Linux Bridge

## Step 1: Open the Network Page

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Click **Network**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Open the Create Menu

1. Click **Create**.
2. Select **Linux Bridge**.

The Create: Linux Bridge window opens.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Configure the Linux Bridge

Configure the required settings.

Typical options include:

- Bridge Name
- IPv4/CIDR
- IPv4 Gateway
- IPv6/CIDR (Optional)
- IPv6 Gateway (Optional)
- Bridge Ports
- VLAN Aware
- Autostart
- Comments (Optional)

Review the configuration before continuing.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Create the Linux Bridge

1. Click **Create**.

The bridge is added to the Network page.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Step 5: Apply the Network Configuration

After creating the bridge, apply the pending network changes.

1. Click **Apply Configuration**.
2. Review the confirmation message.
3. Click **Yes** to apply the changes.

> **Note:** Applying the configuration activates the new bridge. Network connectivity may be briefly interrupted while the changes are applied.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The Linux Bridge appears in the Network page.
- The bridge status is active.
- The correct physical interface is attached.
- The configured IP address is displayed correctly.
- Virtual machines or containers connected to the bridge have network connectivity.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Bridge cannot be created | Verify that the bridge name is unique and all required fields are completed. |
| Physical interface is unavailable | Confirm that the selected network interface exists and is active. |
| Unable to apply the configuration | Review the pending changes for configuration errors before retrying. |
| Network connectivity is lost | Verify the IP address, gateway, and bridge port configuration. Restore the previous configuration if necessary. |
| VLAN traffic is not working | Ensure that VLAN Aware is enabled on the bridge and that the connected switch port is configured correctly. |

---

# Summary

A Linux Bridge provides network connectivity between virtual workloads and the physical network. By creating and configuring a Linux Bridge, administrators can deploy management, production, storage, or VLAN-enabled networks for virtual machines and containers.
