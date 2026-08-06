# Create User

---

## Overview

Creating a user allows an administrator to grant access to VM2Cloud. Each user authenticates through an authentication realm and can be assigned roles and permissions to control access to cluster resources.

A newly created user does not automatically have administrative privileges. Access is determined by the permissions assigned after the user is created.

---

## When to Use

Create a user when you need to:

- Provide access to a new administrator.
- Allow operators to manage virtual infrastructure.
- Create accounts for developers or support engineers.
- Grant users access to specific resources.
- Configure users for API access.

---

## Prerequisites

Before creating a user, ensure that:

- You have administrator privileges.
- The required authentication realm exists.
- The username follows your organization's naming convention.
- The required roles and permissions have been identified.

---

# Create a User

## Step 1: Open the Users Page

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **Users**.

The Users page displays all configured user accounts.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Open the Create User Window

1. Click **Add**.

The Add User window opens.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Configure the User

Enter the required user information.

Typical fields include:

- User Name
- Realm
- Password
- Confirm Password
- Expire (Optional)
- First Name (Optional)
- Last Name (Optional)
- Email (Optional)
- Comment (Optional)
- Enable Account

Review the information before continuing.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Create the User

1. Click **Add**.

The user is created and appears in the Users list.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Assign Permissions

Creating a user does not automatically grant access to VM2Cloud resources.

After creating the user:

1. Assign the user to a group (optional).
2. Assign the required role.
3. Configure the appropriate permissions for the required resource.

> **Note:** User permissions are documented in **Assign-Permissions.md**.

---

# Verification

Verify the following:

- The new user appears in the Users list.
- The correct authentication realm is assigned.
- The account status is **Enabled**.
- The user can successfully authenticate.
- The assigned permissions allow access to the intended resources.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| User already exists | Choose a unique username within the selected authentication realm. |
| Unable to create the user | Verify that all required fields have been completed. |
| User cannot log in | Confirm that the account is enabled and the correct authentication realm is selected during login. |
| User has no access to resources | Assign the appropriate roles and permissions after creating the user. |
| Password is rejected | Ensure that the password meets the configured password policy, if applicable. |

---

# Summary

Creating a user adds a new account to VM2Cloud and allows an individual to authenticate using the selected authentication realm. After the user is created, appropriate roles and permissions should be assigned to provide access to the required resources while maintaining the principle of least privilege.
