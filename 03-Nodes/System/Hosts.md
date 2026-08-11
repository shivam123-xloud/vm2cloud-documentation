# Hosts

---

## Overview

The **Hosts** page allows administrators to manage the local hostname resolution entries for a VM2Cloud node. These entries are stored in the node's hosts file and are used to resolve hostnames before querying a DNS server.

Host entries are commonly used for local communication, cluster nodes, and environments where DNS is unavailable or where specific hostname-to-IP mappings are required.

---

## When to Use

Use the **Hosts** page to:

- View the current hosts configuration.
- Add hostname-to-IP address mappings.
- Modify existing host entries.
- Remove obsolete host entries.
- Troubleshoot hostname resolution issues.

---

## Prerequisites

Before modifying the Hosts configuration, ensure that:

- You are logged in to the VM2Cloud web interface.
- You have administrative privileges.
- The selected node is online.
- You know the correct hostname and IP address to configure.

---

# View Host Entries

## Step 1: Open the Hosts Page

1. Log in to the VM2Cloud web interface.
2. Select the required node.
3. Expand **System**.
4. Select **Hosts**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Existing Entries

The Hosts page displays all configured hostname mappings.

Typical information includes:

- IP Address
- Hostname
- Aliases (if configured)

Review the entries to verify that they are correct.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Add a Host Entry

## Step 1: Open the Add Host Dialog

1. On the **Hosts** page, click **Create**.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the Host Entry

Enter the required information.

Typical fields include:

- IP Address
- Hostname
- Aliases (optional)

After completing the required fields, click **Create**.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Edit a Host Entry

## Step 1: Select the Entry

1. Select the required host entry.
2. Click **Edit**.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Step 2: Update the Configuration

Modify the required information.

Typical fields include:

- IP Address
- Hostname
- Aliases

Click **OK** to save the changes.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Remove a Host Entry

## Step 1: Select the Entry

1. Select the host entry to remove.
2. Click **Remove**.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

## Step 2: Confirm the Removal

1. Review the selected entry.
2. Click **Yes** to confirm.

The host entry is removed from the local hosts configuration.

---

### Screenshot 8

```text
[ Place Screenshot Here ]
```

---

## Typical Uses

Host entries are commonly used for:

- Cluster node communication.
- Local hostname resolution.
- Internal servers.
- Storage servers.
- Temporary hostname mappings during maintenance.
- Environments without DNS.

---

## Best Practices

- Use valid IP addresses.
- Ensure hostnames are unique.
- Remove unused host entries.
- Verify hostname resolution after making changes.
- Use DNS for large environments whenever possible.

---

# Verification

Verify the following:

- The host entry appears in the Hosts page.
- The hostname resolves to the correct IP address.
- Existing entries remain unchanged.
- Applications can communicate using the configured hostname.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Hostname cannot be resolved | Verify the IP address and hostname are configured correctly. |
| Duplicate hostname | Ensure each hostname is unique within the hosts configuration. |
| Incorrect IP address | Edit the entry and specify the correct IP address. |
| Unable to save changes | Verify that your account has administrative privileges. |

---

# Related Documentation

- System Overview
- DNS
- Network Management
- Cluster Overview

---

# Summary

The **Hosts** page allows administrators to manage local hostname-to-IP address mappings on a VM2Cloud node. Properly configured host entries help ensure reliable hostname resolution for cluster communication, internal services, and environments where DNS is unavailable or requires supplemental local mappings.
