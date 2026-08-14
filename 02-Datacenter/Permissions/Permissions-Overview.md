# Permissions Overview

---

## Overview

User and Permission Management allows administrators to control who can access VM2Cloud VE and what actions they are allowed to perform. By creating users, organizing them into groups, assigning roles, and configuring permissions, administrators can securely manage access to cluster resources.

VM2Cloud VE supports both local and external authentication methods, enabling organizations to integrate with existing identity management systems while maintaining secure administrative access.

Proper user and permission management helps protect the virtualization environment by ensuring that users have access only to the resources and operations required for their responsibilities.

---

## When to Use

Use User and Permission Management to:

- Create and manage user accounts.
- Configure authentication methods.
- Organize users into groups.
- Assign roles to users and groups.
- Control access to nodes, virtual machines, containers, storage, and other resources.
- Configure API access for automation.
- Enable additional authentication methods such as Two-Factor Authentication (2FA).

---

## Prerequisites

Before managing users and permissions, ensure that:

- You have administrator privileges.
- The VM2Cloud VE cluster is accessible.
- Authentication services are available if using external authentication.
- You understand the required access level for each user.

---

# Access User and Permission Management

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Click **Permissions**.

The Permissions section provides access to all user and permission management features.

---


![Access User and Permission Management](images/permission-page.png)


---

# Components of User and Permission Management

The Permissions section includes the following components.

| Component | Description |
|-----------|-------------|
| [Users](Users.md) | Create, edit, and delete user accounts. |
| [Groups](Groups.md) | Organize users into logical groups for easier permission management. |
| [Pools](Pools.md) | Group guests and storage so they can be permissioned as one unit. |
| [Roles](Roles.md) | Named sets of privileges. |
| [Permissions](Assign-Permissions.md) | Bind a role to a user or group on a path. |
| [Realms](Authentication-Realms.md) | Configure local or external authentication methods. |
| [API Tokens](API-Tokens.md) | Credentials for automation and API access. |
| [Two-Factor Authentication](Two-Factor-Authentication.md) | A second authentication factor for accounts. |

---


![Components of User and Permission Management](images/permission-components.png)


---

# How Access Actually Works

A **permission** is not a property of a user. It is a binding of three things at once:

```text
   WHO                WHAT                 WHERE
   ───                ────                 ─────
 user or group   +    role       on a      path
 alice@pve            PVEVMAdmin           /pool/finance
```

Read as: *alice@pve has PVEVMAdmin on /pool/finance.*

All three are required. A role by itself grants nothing until it is bound to someone on a path. This is why "assign a role to a user" is an incomplete instruction — the path is what decides where the privileges apply.

## Paths

Resources form a hierarchy, and permissions are granted on paths within it:

```text
/                          the whole environment
├── /nodes/<node>          one server
├── /vms/<vmid>            one guest
├── /storage/<storage>     one storage
├── /pool/<pool>           everything in a pool
├── /access                user and realm management
└── /sdn                   software-defined networking
```

A permission granted on a path applies to everything beneath it when **Propagate** is enabled. A role on `/` therefore reaches every guest, node, and storage in the environment.

See [Assign Permissions](Assign-Permissions.md) for the full path reference.

## Authentication is separate from authorization

The realm decides **whether** someone can log in. Permissions decide **what they can do** once in.

```text
Realm  →  authenticates the account
Permission (who + role + path)  →  authorizes the action
```

A user with a valid account and no permissions logs in successfully and sees an empty resource tree. That is working as designed, not a fault.

## root@pam is outside the model

The `root@pam` account always has full access and **cannot be restricted by permissions**. Control it by controlling the password, not by configuring roles. See [Assign Permissions](Assign-Permissions.md).

---

# Security Best Practices

When managing users and permissions:

- Assign only the permissions required for a user's responsibilities.
- Use groups to simplify permission management.
- Avoid sharing user accounts.
- Enable Two-Factor Authentication for administrative users.
- Review user accounts and permissions regularly.
- Remove unused accounts and API tokens.

---

# Verification

Verify the following:

- Users can successfully authenticate.
- Groups and roles are configured correctly.
- Permissions provide the expected level of access.
- Users can access only the resources assigned to them.
- Administrative accounts are secured with appropriate authentication methods.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| User cannot log in | Verify the username, password, and selected authentication realm. |
| User cannot access resources | Confirm that the appropriate role and permissions have been assigned. |
| Permission changes are not effective | Verify that the permissions are assigned to the correct resource path and that there are no conflicting permissions. |
| Authentication fails | Check the configuration of the selected authentication realm. |
| API authentication fails | Verify that the API token is valid and has the required permissions. |

---

# Summary

The User and Permission Management section provides centralized control over authentication, authorization, and access management in VM2Cloud VE. By properly configuring users, groups, roles, permissions, authentication realms, and API tokens, administrators can secure the environment while ensuring that users have access only to the resources they need.
