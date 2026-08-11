# Permissions Troubleshooting

---

## Overview

This document provides solutions for common issues related to user accounts, authentication, groups, roles, permissions, API Tokens, and Two-Factor Authentication in VM2Cloud.

Use this guide to diagnose and resolve access-related problems before contacting system support.

---

# User Authentication Issues

## Problem

Unable to log in to VM2Cloud.

### Possible Causes

- Incorrect username or password.
- Incorrect authentication realm selected.
- User account is disabled.
- User account has expired.
- Authentication service is unavailable.

### Resolution

1. Verify the username and password.
2. Confirm that the correct authentication realm is selected.
3. Ensure the user account is enabled.
4. Check whether the account has expired.
5. Verify that the authentication server is available.

---

# User Cannot Access Resources

## Problem

The user can successfully log in but cannot access nodes, storage, virtual machines, or containers.

### Possible Causes

- No permissions assigned.
- Incorrect role assigned.
- Permission assigned to the wrong resource path.
- Permission inheritance is not configured correctly.

### Resolution

1. Verify that a permission has been assigned.
2. Confirm that the correct role is assigned.
3. Review the permission path.
4. Check whether **Propagate** is enabled when required.
5. Verify whether another permission overrides the expected access.

---

# Permission Changes Are Not Applied

## Problem

Permission changes do not appear to take effect.

### Possible Causes

- Incorrect permission path.
- Cached browser session.
- User belongs to multiple groups.
- Conflicting permissions.

### Resolution

1. Verify the assigned resource path.
2. Sign out and sign back in.
3. Review group memberships.
4. Verify inherited permissions.
5. Confirm that the correct role has been assigned.

---

# Group Permissions Are Not Working

## Problem

Users do not receive the permissions assigned to their group.

### Possible Causes

- User is not a member of the group.
- Permissions are assigned to another group.
- Incorrect role assignment.

### Resolution

1. Verify group membership.
2. Review group permissions.
3. Confirm that the correct role is assigned.
4. Verify the permission path.

---

# Unable to Create a User

## Problem

The user account cannot be created.

### Possible Causes

- Username already exists.
- Required fields are missing.
- Invalid authentication realm.

### Resolution

1. Use a unique username.
2. Complete all required fields.
3. Verify the selected authentication realm.
4. Retry the operation.

---

# Unable to Delete a User

## Problem

The user account cannot be removed.

### Possible Causes

- Insufficient privileges.
- Incorrect user selected.

### Resolution

1. Verify administrator privileges.
2. Confirm that the correct user is selected.
3. Retry the operation.

---

# API Token Authentication Fails

## Problem

Applications cannot authenticate using an API Token.

### Possible Causes

- Invalid API Token.
- Token expired.
- Token disabled.
- Insufficient permissions.

### Resolution

1. Verify that the token is enabled.
2. Confirm that the token has not expired.
3. Verify that the associated user has the required permissions.
4. Generate a new API Token if necessary.

---

# Two-Factor Authentication Issues

## Problem

User cannot complete Two-Factor Authentication.

### Possible Causes

- Incorrect verification code.
- Time synchronization issue.
- Authentication device unavailable.
- Browser compatibility issue.

### Resolution

1. Verify the verification code.
2. Synchronize the device time.
3. Retry using the configured authentication device.
4. Use recovery keys if available.
5. Reconfigure Two-Factor Authentication if necessary.

---

# Authentication Realm Issues

## Problem

Users cannot authenticate using an external authentication service.

### Possible Causes

- Directory server unavailable.
- Incorrect server configuration.
- Network connectivity issue.
- Invalid bind credentials.

### Resolution

1. Verify network connectivity.
2. Confirm the authentication server is online.
3. Review the realm configuration.
4. Verify the configured credentials.
5. Test authentication again.

---

# Common Administrative Checks

When troubleshooting user and permission issues, verify the following:

- The user account exists.
- The correct authentication realm is selected.
- The account is enabled.
- The account has not expired.
- The user belongs to the correct group.
- The appropriate role is assigned.
- Permissions are assigned to the correct resource path.
- Permission propagation is configured correctly.
- API Tokens are enabled and valid.
- Two-Factor Authentication is configured correctly.

---

# Best Practices

- Follow the principle of least privilege.
- Assign permissions to groups instead of individual users whenever possible.
- Regularly review user accounts and permissions.
- Remove unused accounts and API Tokens.
- Enable Two-Factor Authentication for privileged users.
- Document all administrative permission changes.

---

# Summary

Most user and permission issues are caused by incorrect authentication settings, missing permissions, or misconfigured roles. By verifying user accounts, authentication realms, group memberships, roles, permissions, API Tokens, and Two-Factor Authentication settings, administrators can quickly identify and resolve access-related problems while maintaining a secure VM2Cloud environment.
