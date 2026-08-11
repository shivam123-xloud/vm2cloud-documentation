# Datacenter Options

---

## Overview

The **Cluster Options** page allows administrators to configure cluster-wide settings that affect the overall behavior of the VM2Cloud cluster.

These settings are shared across all cluster nodes and help define how the cluster operates. Changes made through the Cluster Options page are automatically synchronized to all nodes in the cluster.

Cluster options should only be modified by administrators who understand their impact on the cluster environment.

---

## When to Use

Use the **Cluster Options** page to:

- Review current cluster settings.
- Modify cluster-wide configuration.
- Configure migration-related settings (when available).
- Adjust cluster behavior based on operational requirements.

---

## Prerequisites

Before modifying cluster options, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have administrative privileges.
- The cluster is healthy and has quorum.
- All nodes are online whenever possible.

---

# View Cluster Options

## Step 1: Open the Cluster

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Select **Cluster**.
4. Open the **Options** tab.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Cluster Options

The **Options** page displays the available cluster-wide settings.

Typical information includes:

- Option Name
- Current Value
- Description

Review the current configuration before making any changes.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Edit a Cluster Option

1. Select the option you want to modify.
2. Click **Edit**.
3. Configure the required value.
4. Click **OK** to save the changes.

Changes are applied to the cluster configuration and synchronized across all nodes.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Available Cluster Options

> **Verify:** Capture the full list of settings on the Datacenter → Options panel and
> document each one.

Typical cluster options include:

| Option | Description |
|---------|-------------|
| Migration Settings | Configures the default behavior for virtual machine migrations when supported. |
| Cluster Configuration | Displays cluster-wide configuration values managed by VM2Cloud. |
| Other Version-Specific Options | Additional options may be available depending on the VM2Cloud release. |

---

## Best Practices

- Review the purpose of an option before modifying it.
- Change only one option at a time.
- Verify cluster health before making configuration changes.
- Test configuration changes in a non-production environment whenever possible.
- Record significant configuration changes for future reference.

---

# Verification

Verify the following after modifying a cluster option:

- The updated value is displayed in the **Options** page.
- No configuration errors are reported.
- All cluster nodes remain online.
- Cluster services continue operating normally.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Unable to edit an option | Verify that your user account has administrative privileges. |
| Changes cannot be saved | Confirm that the cluster has quorum and all required services are running. |
| Option is unavailable | Some options are version-specific and may not be available in every VM2Cloud deployment. |
| Unexpected cluster behavior after a change | Review the modified option and restore the previous configuration if necessary. |

---

# Related Documentation

- Cluster Overview
- Create Cluster
- Join Node to Cluster
- Cluster Quorum
- Cluster File System
- Cluster Certificates
- Cluster Troubleshooting

---

# Summary

The **Cluster Options** page provides centralized management of cluster-wide configuration settings. By carefully reviewing and modifying these options, administrators can adjust cluster behavior while maintaining consistent configuration across all VM2Cloud nodes.
