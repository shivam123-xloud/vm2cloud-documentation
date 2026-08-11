# Cloud-Init

---

## Overview

**Cloud-Init** configures a virtual machine automatically the first time it boots — setting the hostname, creating a user, installing an SSH key, and applying network configuration — without anyone logging in to do it manually.

It turns a generic cloud image into a ready-to-use machine. Instead of installing an operating system from an ISO and answering an installer's questions, you clone a prepared template, fill in the Cloud-Init fields, and start it.

The **Cloud-Init** tab appears on a virtual machine when a Cloud-Init drive is attached.

### Why it matters

Deploying ten identical VMs from an ISO means running the installer ten times. With Cloud-Init you prepare one template, then clone it ten times and set a hostname, an IP address, and an SSH key on each — a few seconds per machine, with no manual installation at all.

### What it needs

Cloud-Init only works when both of these are true:

1. The guest operating system image has the Cloud-Init package installed and enabled. Vendor cloud images normally do; an image you installed yourself from an ISO normally does not.
2. A **CloudInit Drive** is attached to the virtual machine, so the settings can be presented to the guest at boot.

If either is missing, the fields can be filled in and saved, and the guest will simply ignore them.

---

## When to Use

Use Cloud-Init when:

* Deploying virtual machines from cloud images.
* Creating many similar machines that differ only in hostname, address, and credentials.
* Automating first-boot configuration.
* Injecting SSH keys instead of setting passwords manually.
* Assigning static addresses at deployment time.
* Building a template that others will clone.

Do not use it for machines installed manually from an ISO unless you have installed and enabled the Cloud-Init package inside the guest yourself.

---

## Prerequisites

Before using Cloud-Init, ensure that:

* You have administrator privileges, or permissions on the virtual machine.
* The guest image supports Cloud-Init — a vendor cloud image, or an image you have prepared.
* A **CloudInit Drive** is attached to the machine.
* You have the SSH public key you intend to install.
* You know the network configuration the machine should use.
* The machine has **not yet been started**, or you understand that most settings only apply on first boot.

---

# Procedure

## Step 1: Attach a Cloud-Init Drive

Skip this if the machine already has one — check whether the **Cloud-Init** tab is present.

1. Select the virtual machine.
2. Click **Hardware**.
3. Click **Add**.
4. Select **CloudInit Drive**.
5. Choose the storage for it.
6. Click **Add**.

The **Cloud-Init** tab appears.

---

### Screenshot 1

**Adding a CloudInit Drive**

```text
[ Place Screenshot Here ]
```

> **Capture:** VM → Hardware → Add → CloudInit Drive, showing the storage selection
> dialog.

---

## Step 2: Open the Cloud-Init Tab

1. Select the virtual machine.
2. Click **Cloud-Init**.

The configurable settings are listed with their current values.

---

### Screenshot 2

**Cloud-Init Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** VM → Cloud-Init, showing all available settings with their current
> values and the **Edit** and **Regenerate Image** controls.

---

## Step 3: Set the User Account

1. Select **User**.
2. Click **Edit**.
3. Enter the username to create in the guest.
4. Click **OK**.

This is the account Cloud-Init creates on first boot. On many vendor images a default account already exists; setting this creates or configures the one you specify.

---

## Step 4: Set the Password or SSH Key

Prefer the SSH key. Passwords set through Cloud-Init are stored in the machine's configuration and are visible to anyone who can read it.

**To add an SSH public key:**

1. Select **SSH public key**.
2. Click **Edit**.
3. Paste the public key, or load it from a file.
4. Click **OK**.

**To set a password:**

1. Select **Password**.
2. Click **Edit**.
3. Enter the password.
4. Click **OK**.

> **Warning:** Passwords configured through Cloud-Init are stored in the virtual machine's configuration. Anyone with permission to view that configuration can retrieve them. Use SSH keys for anything that matters.

---

### Screenshot 3

**Setting the SSH Public Key**

```text
[ Place Screenshot Here ]
```

> **Capture:** The SSH public key edit dialog on the Cloud-Init tab.

---

## Step 5: Configure Networking

1. Select the network interface entry, for example **IP Config (net0)**.
2. Click **Edit**.
3. Choose the addressing method:
   - **DHCP** — the machine obtains an address automatically.
   - **Static** — enter the address in CIDR form, such as `192.168.1.50/24`, and the gateway.
4. Configure IPv6 the same way if required.
5. Click **OK**.

Enter the address with its prefix. A static address without a correct prefix and gateway leaves the machine unreachable.

---

### Screenshot 4

**IP Configuration Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The IP Config edit dialog on the Cloud-Init tab, showing the DHCP and
> static options with the IPv4 and IPv6 fields.

---

## Step 6: Set DNS

1. Select **DNS domain**.
2. Click **Edit**, enter the search domain, click **OK**.
3. Select **DNS servers**.
4. Click **Edit**, enter the nameservers, click **OK**.

If left unset, the host's own DNS settings are normally used.

---

## Step 7: Regenerate the Image

After changing settings on a machine that already exists, the Cloud-Init image must be regenerated so the new values are presented at next boot.

1. Click **Regenerate Image**.
2. Wait for the task to complete.

> **Verify:** Confirm the exact label of this control and whether regeneration happens
> automatically when settings are saved.

---

### Screenshot 5

**Regenerating the Cloud-Init Image**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Cloud-Init tab with the Regenerate Image action, and the resulting
> task output.

---

## Step 8: Start the Machine and Verify

1. Click **Start**.
2. Open the **Console** and watch the boot.
3. Confirm the hostname is correct.
4. Confirm the network configuration applied.
5. Connect over SSH using the configured key.

First boot takes longer than subsequent boots, because Cloud-Init is doing its work.

---

### Screenshot 6

**First Boot With Cloud-Init**

```text
[ Place Screenshot Here ]
```

> **Capture:** The VM console during first boot, showing Cloud-Init output as it applies
> the configuration.

---

# Configuration / Options

| Option | Description |
|---|---|
| **User** | Username created in the guest on first boot. |
| **Password** | Password for that user. Stored in the VM configuration — prefer an SSH key. |
| **DNS domain** | Search domain applied inside the guest. |
| **DNS servers** | Nameservers applied inside the guest. |
| **SSH public key** | Public key installed for the user. The recommended authentication method. |
| **Upgrade packages** | Whether the guest runs a package upgrade on first boot. Lengthens first boot. |
| **IP Config (netN)** | Address configuration per interface — DHCP or a static address with gateway, for IPv4 and IPv6. |

> **Verify:** Capture the complete Cloud-Init tab and confirm the exact setting names
> and which options are present in this deployment.

---

# First Boot Only

Most Cloud-Init settings are applied **once**, on the machine's first boot. Changing them afterwards and rebooting usually has no effect, because the guest records that initialisation has already run.

This trips people up constantly: the fields are edited, the machine is rebooted, and nothing changes.

To apply a change afterwards you must either reconfigure inside the guest directly, reset the Cloud-Init state within the guest so it runs again, or clone the template afresh and configure the new machine before its first boot.

Get the configuration right before the first start.

---

# Using Cloud-Init With Templates

The usual workflow:

1. Create a virtual machine from a vendor cloud image.
2. Attach a CloudInit Drive.
3. Configure any settings common to every deployment, such as the SSH key and DNS.
4. Do **not** start the machine.
5. Convert it to a template.
6. Clone the template for each new machine.
7. On each clone, set the hostname and address.
8. Start the clone.

Each clone gets its own configuration on its own first boot. See [Clone Virtual Machine](Clone-Virtual-Machine.md).

---

# Verification

Verify the following:

* The **Cloud-Init** tab is present, confirming a drive is attached.
* Settings show the intended values.
* The image was regenerated after any change.
* On first boot, the console shows Cloud-Init running.
* The hostname inside the guest is correct.
* The network configuration applied, and the address is reachable.
* SSH authentication with the configured key succeeds.
* DNS resolution works inside the guest.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Cloud-Init tab missing | No CloudInit Drive is attached. Add one under Hardware. |
| Settings appear ignored | The guest image does not have Cloud-Init installed. Vendor cloud images do; ISO installs usually do not. |
| Changes have no effect after reboot | Most settings apply on first boot only. Reconfigure inside the guest, or clone the template again. |
| Machine unreachable after static configuration | Check the CIDR prefix and gateway. An address without a correct prefix leaves the machine unreachable. |
| SSH key rejected | Confirm the **public** key was pasted, complete and on one line, and that you are connecting as the configured user. |
| Password does not work | Some images disable password authentication over SSH by default. Use the key, or enable password login inside the guest. |
| First boot very slow | **Upgrade packages** is enabled, so the guest is updating during boot. |
| Clone reuses the template's hostname | The hostname was not set on the clone before its first start. |
| Network settings not applied | The image was not regenerated after editing. |

---

# Best Practices

- Use SSH keys, not passwords. Passwords are stored in the VM configuration in readable form.
- Configure everything before the first start. Most settings cannot be changed afterwards.
- Build a template with the shared settings, then set only hostname and address per clone.
- Always enter static addresses in CIDR form with the correct gateway.
- Regenerate the image after any change to an existing machine.
- Watch the first boot on the console rather than assuming it worked.
- Leave **Upgrade packages** off for fast deployment; patch afterwards through your normal process.
- Use vendor cloud images rather than trying to retrofit Cloud-Init onto an ISO install.
- Document which template a machine came from and what was set on it.

---

# Related Documentation

- [Create Virtual Machine](Create-Virtual-Machine.md)
- [Manage VM Hardware](Manage-VM-Hardware.md)
- [Clone Virtual Machine](Clone-Virtual-Machine.md)
- [VM Options](VM-Options.md)
- [VM Console](VM-Console.md)
- [VM Troubleshooting](VM-Troubleshooting.md)
- [Upload Content](../02-Datacenter/Storage/Upload-Content.md)

---

# Summary

Cloud-Init configures a virtual machine automatically on first boot — user, SSH key, network, and DNS — turning a generic cloud image into a ready machine without manual installation. It requires both a guest image with Cloud-Init installed and a CloudInit Drive attached to the machine; without either, the settings are saved and silently ignored.

The critical constraint is that most settings apply **only on first boot**. Get the configuration right before you start the machine, because editing afterwards and rebooting will not apply it. The natural pattern is a configured template, cloned per machine, with hostname and address set on each clone before its first start.
