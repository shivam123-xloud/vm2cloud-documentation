# API Tokens

---

## Overview

API Tokens provide a secure way for applications, scripts, and automation tools to authenticate with the VM2Cloud API without using a user's password. Each API Token is associated with an existing user account and inherits only the permissions assigned to that user.

API Tokens are commonly used for infrastructure automation, monitoring systems, backup software, CI/CD pipelines, and third-party integrations.

> **Note:** An API Token cannot be used unless the associated user has the required permissions.

---

## When to Use

Use API Tokens when you need to:

- Automate VM2Cloud administration.
- Authenticate API requests securely.
- Integrate third-party applications.
- Execute automation scripts.
- Avoid using user passwords in scripts.

---

## Prerequisites

Before creating an API Token, ensure that:

- You have administrator privileges.
- The user account already exists.
- The user has the required permissions.
- The application or script supports API Token authentication.

---

# Access the API Tokens Page

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **API Tokens**.

The API Tokens page displays all configured API Tokens.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

# Create an API Token

## Step 1: Open the Create API Token Window

1. Click **Add**.

The Add API Token window opens.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the API Token

Enter the required information.

Typical fields include:

- User
- Token ID
- Expire (Optional)
- Privilege Separation
- Comment (Optional)

Review the configuration before continuing.

### Privilege Separation

When enabled, the API Token uses only the permissions explicitly assigned to the token.

When disabled, the token inherits all permissions assigned to the associated user.

> **Recommendation:** Enable **Privilege Separation** whenever possible to follow the principle of least privilege.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 3: Create the API Token

1. Click **Add**.

VM2Cloud generates the API Token.

> **Important:** Copy and securely store the generated API Token immediately. After the dialog is closed, the token value cannot be viewed again.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# View API Tokens

1. Open **Datacenter** → **Permissions** → **API Tokens**.

The page displays information such as:

- User
- Token ID
- Enabled Status
- Expiration Date
- Privilege Separation

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

# Enable or Disable an API Token

1. Select the required API Token.
2. Click **Edit**.
3. Enable or disable the token.
4. Click **OK**.

Disabling an API Token immediately prevents it from being used for authentication.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Delete an API Token

> **Warning:** Deleting an API Token permanently removes it. Applications or scripts using the token will no longer be able to authenticate.

## Steps

1. Select the required API Token.
2. Click **Remove**.
3. Confirm the operation.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The API Token appears in the API Tokens list.
- The associated user is correct.
- The token status is **Enabled**.
- Applications authenticate successfully using the token.
- The assigned permissions provide the expected level of access.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Authentication fails | Verify that the API Token is valid, enabled, and associated with the correct user. |
| Access denied | Ensure that the associated user or the API Token has the required permissions. |
| Token cannot be viewed | API Tokens are displayed only once during creation. Create a new token if the original value has been lost. |
| Token has expired | Create a new API Token or update the expiration date if permitted. |
| Automation no longer works | Verify that the API Token has not been disabled or deleted. |

---

# Security Best Practices

- Create separate API Tokens for different applications.
- Enable **Privilege Separation** whenever possible.
- Grant only the permissions required by the application.
- Store API Tokens securely.
- Rotate API Tokens periodically.
- Delete unused or expired API Tokens.

---

# Summary

API Tokens provide a secure authentication method for automation and third-party integrations without exposing user passwords. By assigning appropriate permissions and following security best practices, administrators can safely integrate VM2Cloud with external applications and automation workflows.
