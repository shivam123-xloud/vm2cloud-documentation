# Hosts

---

## Overview

The **Hosts** page allows administrators to manage the local hostname resolution entries for a VM2Cloud VE node. These entries are stored in the node's hosts file and are used to resolve hostnames before querying a DNS server.

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

- You are logged in to the VM2Cloud VE web interface.
- You have administrative privileges.
- The selected node is online.
- You know the correct hostname and IP address to configure.

---

# View Host Entries

## Step 1: Open the Hosts Page

1. Log in to the VM2Cloud VE web interface.
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

Each entry maps an address to one or more names:

| Element | Purpose |
|---|---|
| **IP address** | The address the names resolve to. |
| **Hostname** | The primary fully qualified name for that address. |
| **Aliases** | Additional short names for the same address. |

The entry resolving **this node's own hostname** is the one that matters. The node reads its own address from it for cluster communication and certificate generation, so it must match the address actually configured on the management interface.

> **Verify:** Confirm how this panel presents the file — as an editable plain-text view of
> `/etc/hosts` with a Save control, or as a table with per-entry Add and Edit dialogs.
> The procedure below assumes per-entry dialogs; if it is a text editor, these steps
> need rewriting as "edit the line and save".

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

| Field | What to enter |
|---|---|
| **IP address** | The address being named. |
| **Hostname** | The fully qualified name, such as `node1.example.com`. |
| **Aliases** | Optional short names, such as `node1`. |

Give every cluster node an entry containing both its fully qualified name and its short name. Cluster communication resolves node names locally, so a missing or wrong entry produces join failures and certificate errors that look unrelated to DNS.

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

| Field | Notes |
|---|---|
| **IP address** | Change only when the node or host has genuinely moved. |
| **Hostname** | The fully qualified name. |
| **Aliases** | Short names. |

> **Warning:** Changing the address on this node's own entry without also changing the interface configuration leaves the two disagreeing. The node then advertises an address it does not hold, which breaks cluster join, certificate generation, and the cluster join information. Confirm with `hostname -i` after any change to this node's entry.

Click **OK** to save the changes.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Remove a Host Entry

> **Warning:** Do not remove the entry that resolves this node's own hostname. The
> node determines its own address from that entry, and removing it breaks cluster
> communication and certificate generation. If you are unsure which entry that is,
> check `hostname -i` before changing anything.

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

The **Hosts** page allows administrators to manage local hostname-to-IP address mappings on a VM2Cloud VE node. Properly configured host entries help ensure reliable hostname resolution for cluster communication, internal services, and environments where DNS is unavailable or requires supplemental local mappings.
