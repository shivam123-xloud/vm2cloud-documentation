# Repositories

---

## Overview

The **Repositories** page allows administrators to manage the software repositories configured on a VM2Cloud VE node. Repositories provide access to operating system packages, VM2Cloud VE updates, security patches, and other software components.

Keeping repositories properly configured ensures that the node can receive updates and install supported packages.

---

## When to Use

Use the **Repositories** page to:

- View configured repositories.
- Enable or disable repositories.
- Add custom repositories.
- Verify repository status.
- Troubleshoot package update issues.

---

## Prerequisites

Before managing repositories, ensure that:

- You are logged in to the VM2Cloud VE web interface.
- You have administrative privileges.
- The selected node is online.
- The node has internet access or connectivity to the configured repository server.

---

# View Repositories

## Step 1: Open the Repositories Page

1. Log in to the VM2Cloud VE web interface.
2. Select the required node.
3. Select **Repositories**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Repository Configuration

The page displays the configured software repositories.

| Column | What it shows |
|---|---|
| **Enabled** | Whether the repository is currently used when checking for updates. |
| **Component** | Which set of packages the repository provides. |
| **Origin** | Who publishes it. |
| **Status** | Whether the repository is reachable and correctly configured. |

The panel also reports whether the node's overall repository configuration is suitable for its intended use — for example warning when a node is configured for one release channel but expected to be on another.

Review the repository list to verify the current configuration.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Add a Repository

> **Note**
>
> This option is available only if your VM2Cloud VE deployment supports adding custom repositories.

## Step 1: Open the Add Repository Dialog

1. Click **Add**.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the Repository

Provide the required information.

> **Verify:** Confirm how repositories are added in this deployment — whether **Add**
> offers a list of known repositories to select from, or free-text fields for URL,
> distribution, and component. The fields below assume free-text entry; if it is a
> selection list, this step should be rewritten as "choose the repository from the list".

| Field | What to enter |
|---|---|
| **Repository URL** | The base address the packages are served from. |
| **Distribution** | The release the repository targets. Must match the node's release. |
| **Component** | Which package set to enable from that repository. |

> **Warning:** A repository targeting a different release than the node runs can offer packages that are incompatible with the installed system. Upgrading from a mismatched repository can leave the node unable to boot or its services unable to start.

Click **Add** to save the configuration.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Enable or Disable a Repository

## Step 1: Select a Repository

1. Select the repository.
2. Click **Enable** or **Disable**, depending on its current status.

The repository status is updated immediately.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Common Repository Types

Depending on your VM2Cloud VE deployment, repositories may include:

| Repository Type | Purpose |
|-----------------|---------|
| Enterprise | Provides stable, enterprise-tested updates. |
| No-Subscription | Provides updates for environments without an enterprise subscription. |
| VM2Cloud VE Repository | Provides VM2Cloud VE-specific packages and updates. |
| Debian Repository | Provides Debian operating system packages. |
| Custom Repository | Provides packages from an internal or third-party source. |

---

## Best Practices

- Enable only the repositories required for your environment.
- Keep repository configurations up to date.
- Verify repository connectivity before performing updates.
- Avoid mixing incompatible repository sources.
- Test new repositories in a non-production environment before deployment.

---

# Verification

Verify the following:

- The required repositories are listed.
- Repository status is correct.
- Repository URLs are reachable.
- Package updates complete successfully.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Repository is unreachable | Verify network connectivity and the repository URL. |
| Package updates fail | Ensure the repository is enabled and accessible. |
| Repository not listed | Refresh the page or verify the repository configuration. |
| Invalid repository configuration | Review the repository details and correct any incorrect values. |

---

# Related Documentation

- Update Node
- Subscription
- System Overview
- Node Troubleshooting

---

# Summary

The **Repositories** page allows administrators to manage the software sources used by a VM2Cloud VE node. Proper repository configuration ensures reliable access to software updates, security patches, and VM2Cloud VE packages while maintaining a stable and supported environment.
