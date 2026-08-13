# DNS

---

## Overview

The **DNS** page allows administrators to configure the Domain Name System (DNS) settings used by a VM2Cloud VE node. DNS is responsible for translating hostnames into IP addresses, enabling the node to communicate with other systems using domain names instead of numeric IP addresses.

Proper DNS configuration is essential for software updates, package installation, cluster communication, authentication services, backups, and access to external resources.

---

## When to Use

Use the **DNS** page to:

- Configure DNS servers.
- Configure the search domain.
- Verify the current DNS configuration.
- Update DNS settings after network changes.
- Troubleshoot name resolution issues.

---

## Prerequisites

Before modifying the DNS configuration, ensure that:

- You are logged in to the VM2Cloud VE web interface.
- You have administrative privileges.
- The selected node is online.
- The DNS server addresses are valid and reachable.

---

# View DNS Configuration

## Step 1: Open the DNS Page

1. Log in to the VM2Cloud VE web interface.
2. Select the required node.
3. Expand **System**.
4. Select **DNS**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review the DNS Configuration

The DNS page displays the current DNS settings.

Typical information includes:

- Search Domain
- DNS Server 1
- DNS Server 2
- DNS Server 3

Review the values to ensure they match your network configuration.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Modify DNS Settings

## Step 1: Open the DNS Configuration

1. On the **DNS** page, click **Edit**.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the DNS Settings

Configure the required values.

Typical options include:

- Search Domain
- DNS Server 1
- DNS Server 2
- DNS Server 3

After entering the required values, click **OK**.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Example DNS Configuration

The following example illustrates a typical configuration.

| Setting | Example |
|----------|---------|
| Search Domain | example.local |
| DNS Server 1 | 8.8.8.8 |
| DNS Server 2 | 1.1.1.1 |
| DNS Server 3 | 8.8.4.4 |

Use values appropriate for your environment.

---

## Why DNS Is Important

Correct DNS configuration enables the node to:

- Resolve hostnames.
- Download software updates.
- Access package repositories.
- Communicate with authentication services.
- Connect to backup servers.
- Access remote storage services.
- Communicate with external systems using domain names.

---

## Best Practices

- Configure at least two DNS servers whenever possible.
- Use reliable and reachable DNS servers.
- Configure the correct search domain for your environment.
- Verify DNS functionality after making changes.
- Use internal DNS servers when required by your organization.

---

# Verification

Verify the following:

- The configured DNS servers are displayed correctly.
- The search domain is correct.
- Hostnames can be resolved successfully.
- Network services that depend on DNS operate normally.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Hostnames cannot be resolved | Verify that the configured DNS server addresses are correct and reachable. |
| Incorrect search domain | Update the search domain and apply the changes. |
| Software updates fail due to name resolution | Verify the DNS configuration and test connectivity to the configured DNS servers. |
| Unable to save DNS settings | Confirm that your account has administrative privileges and the node is online. |

---

# Related Documentation

- System Overview
- Hosts
- Network Time (NTP)
- Network Management

---

# Summary

The **DNS** page allows administrators to configure and manage the DNS settings used by a VM2Cloud VE node. Proper DNS configuration ensures reliable hostname resolution and supports services such as software updates, authentication, backups, and communication with other systems.
