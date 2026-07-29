# Manage Cluster Certificates

---

## Overview

Cluster certificates are used to establish secure communication between VM2Cloud nodes and the web interface. Administrators may need to review or regenerate certificates during cluster maintenance, certificate renewal, or when replacing cluster nodes.

---

## When to Use

Manage cluster certificates when you need to:

* Review the current cluster certificate.
* Regenerate certificates after changing a hostname.
* Resolve certificate-related warnings.
* Replace certificates during maintenance.
* Verify certificate information for secure communication.

---

## Prerequisites

Before managing cluster certificates, ensure that:

* You have administrator privileges.
* The cluster is operational.
* All cluster nodes are online.
* A maintenance window is available if certificate regeneration is required.

---

# Procedure

## Step 1: Log in to VM2Cloud

1. Open the VM2Cloud web interface.
2. Sign in using an administrator account.

---

### Screenshot 1

**VM2Cloud Login**

```text
[ Place Screenshot Here ]
```

---

## Step 2: Open Cluster Management

1. Select **Datacenter**.
2. Click **Cluster**.

---

### Screenshot 2

**Cluster Management**

```text
[ Place Screenshot Here ]
```

---

## Step 3: View Cluster Certificate Information

1. Open the certificate or certificate management section.
2. Review the available certificate details.
3. Verify the certificate validity and associated information.

---

### Screenshot 3

**Certificate Information**

```text
[ Place Screenshot Here ]
```

---

## Step 4: Regenerate the Certificate (If Required)

1. Select the option to regenerate or renew the certificate.
2. Review the confirmation message.
3. Confirm the operation.
4. Wait for the process to complete.

---

### Screenshot 4

**Certificate Regeneration**

```text
[ Place Screenshot Here ]
```

---

## Step 5: Refresh the Interface

1. Refresh the VM2Cloud web interface.
2. Verify that the updated certificate information is displayed.

---

### Screenshot 5

**Updated Certificate**

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The certificate information is displayed correctly.
* No certificate warnings are shown.
* The VM2Cloud web interface is accessible.
* Cluster communication continues without errors.

---

# Common Issues

| Issue                                         | Resolution                                                                  |
| --------------------------------------------- | --------------------------------------------------------------------------- |
| Certificate warning displayed                 | Verify the certificate validity and regenerate it if necessary.             |
| Unable to regenerate certificate              | Confirm you have administrator privileges and all cluster nodes are online. |
| Browser continues showing the old certificate | Clear the browser cache or restart the browser session.                     |
| Secure connection warning                     | Verify the certificate details and reconnect to the VM2Cloud interface.     |

---

# Summary

Cluster certificates help secure communication within the VM2Cloud environment. Regularly reviewing certificate information and renewing certificates when required helps maintain secure access to the platform and ensures uninterrupted cluster communication.
