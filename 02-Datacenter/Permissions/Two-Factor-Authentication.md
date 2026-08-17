# Two-Factor Authentication (2FA)

---

## Overview

Two-Factor Authentication (2FA) provides an additional layer of security for VM2Cloud VE user accounts. In addition to entering a username and password, users must verify their identity using a second authentication factor during login.

Enabling 2FA helps protect accounts from unauthorized access, even if a user's password is compromised.

> **Note:** The available 2FA methods depend on your VM2Cloud VE deployment and configuration.

---

## When to Use

Use Two-Factor Authentication when you need to:

- Improve account security.
- Protect administrator accounts.
- Meet organizational security requirements.
- Reduce the risk of unauthorized access.
- Secure remote access to VM2Cloud VE.

---

## Prerequisites

Before configuring 2FA, ensure that:

- You have administrator privileges.
- The user account already exists.
- A supported 2FA method is available.
- The user has access to the required authentication device or application.

---

# Access Two-Factor Authentication

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **Two-Factor Authentication**.

The Two-Factor Authentication page displays the available 2FA configuration options.

---

### Screenshot 1

**Two Factor Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Permissions → Two Factor.

---

# Supported Authentication Methods

Depending on your VM2Cloud VE deployment, one or more of the following authentication methods may be available.

| Authentication Method | Description |
|-----------------------|-------------|
| TOTP | Generates a time-based one-time password using an authenticator application. |
| WebAuthn | Uses a hardware security key or a supported biometric authentication device. |
| Recovery Keys | Allows account recovery if the primary authentication device is unavailable. |

> **Verify:** Capture the full list of second-factor methods offered on the Two Factor panel.

---

### Screenshot 2

**Available Methods**

```text
[ Place Screenshot Here ]
```

> **Capture:** The method list or type dropdown open. **This clears the `Verify` marker
> above** — the method list on this page is inferred.

---

# Configure Two-Factor Authentication

## Step 1: Open the Add 2FA Window

1. Click **Add**.

The Add Two-Factor Authentication window opens.

---

### Screenshot 3

**Add 2FA Window**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add dialog as it opens.

---

## Step 2: Configure the Authentication Method

Select the required authentication method.

Typical options include:

- Authentication Type
- Description (Optional)
- Configuration specific to the selected authentication method

Complete the required configuration.

---

### Screenshot 4

**Method Configuration**

```text
[ Place Screenshot Here ]
```

> **Capture:** The dialog after choosing TOTP, showing the QR code and secret. **Blur
> both** before saving the image.

---

## Step 3: Save the Configuration

1. Click **Add** or **Create**.

The Two-Factor Authentication method is configured for the selected user.

---

### Screenshot 5

**Configured Method**

```text
[ Place Screenshot Here ]
```

> **Capture:** The panel showing the method attached to a user.

---

# Remove Two-Factor Authentication

> **Warning:** Removing 2FA reduces the security of the associated user account.

## Steps

1. Select the configured authentication method.
2. Click **Remove**.
3. Confirm the operation.

The selected authentication method is removed.

---

### Screenshot 6

**Remove a Method**

```text
[ Place Screenshot Here ]
```

> **Capture:** The removal confirmation.

---

# Verification

Verify the following:

- The configured authentication method appears in the Two-Factor Authentication list.
- The user is prompted for a second authentication factor during login.
- Authentication completes successfully.
- Recovery methods function correctly, if configured.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Verification code is rejected | Verify that the device time is synchronized and enter a new code. |
| Hardware security key is not detected | Confirm that the browser and operating system support the selected device. |
| User cannot complete login | Verify that the configured authentication method is functioning correctly. |
| Lost authentication device | Use a configured recovery method or have an administrator remove and reconfigure 2FA. |
| Unable to configure 2FA | Verify that the selected authentication method is supported by your VM2Cloud VE deployment. |

---

# Security Best Practices

- Enable 2FA for all administrator accounts.
- Use hardware security keys whenever possible.
- Store recovery keys securely.
- Do not share authentication devices.
- Review configured authentication methods periodically.
- Remove unused authentication methods.

---

# Summary

Two-Factor Authentication enhances the security of VM2Cloud VE by requiring an additional verification step during user authentication. Enabling 2FA helps protect user accounts from unauthorized access and is recommended for all administrative and privileged accounts.
