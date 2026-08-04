# Create Virtual Machine

---

## Overview

A Virtual Machine (VM) is a software-based computer that runs its own operating system using the resources of a VM2Cloud node. Before a virtual machine can be used, it must be created by defining its hardware configuration and installation source.

The **Create Virtual Machine** wizard guides you through the configuration process, including the virtual machine name, operating system, storage, CPU, memory, and network settings.

---

## When to Use

Create a virtual machine when you need to:

* Deploy a new Linux or Windows server.
* Install an operating system from an ISO image.
* Create a test or development environment.
* Host applications or services.

---

## Prerequisites

Before creating a virtual machine, ensure that:

* A VM2Cloud node is available.
* Storage has been configured.
* A Linux Bridge or other network configuration is available.
* The required ISO image has been uploaded to the storage.
* Sufficient CPU, memory, and storage resources are available.

---

# Create a Virtual Machine

## Step 1: Open the Create Virtual Machine Wizard

1. Log in to the VM2Cloud web interface.
2. Select the node where the virtual machine will be created.
3. Click **Create VM**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: General

Enter the basic virtual machine information.

Typical fields include:

* Node
* VM ID
* Name

After entering the required information, click **Next**.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: OS

Select the installation media.

Typical options include:

* Use CD/DVD Disc Image File (ISO)
* Storage
* ISO Image
* Guest Operating System Type
* Guest Operating System Version

Click **Next**.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: System

Configure the virtual machine firmware and system settings.

Depending on your environment, options may include:

* Graphic Card
* Machine Type
* BIOS
* EFI Disk (if applicable)
* SCSI Controller
* QEMU Agent
* TPM (if applicable)

Review the settings and click **Next**.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Step 5: Disks

Configure the virtual disk.

Typical settings include:

* Bus/Device
* Storage
* Disk Size
* Cache
* Discard
* SSD Emulation

Click **Next** after reviewing the configuration.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Step 6: CPU

Configure the processor settings.

Typical options include:

* Socket(s)
* Cores
* CPU Type

Click **Next**.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

## Step 7: Memory

Specify the memory allocation.

Typical settings include:

* Memory (MB)
* Ballooning Device (if available)

Click **Next**.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

## Step 8: Network

Configure the network adapter.

Typical settings include:

* Bridge
* Model
* VLAN Tag (Optional)
* Firewall (Optional)

Click **Next**.

---

### Screenshot 8

```text
[ Place Screenshot Here ]
```

---

## Step 9: Confirm

Review all selected settings.

Verify:

* VM Name
* ISO Image
* Storage
* Disk Size
* CPU
* Memory
* Network Configuration

If everything is correct, click **Finish**.

---

### Screenshot 9

```text
[ Place Screenshot Here ]
```

---

# Verification

After the wizard completes:

* The virtual machine appears under the selected node.
* The VM status is displayed.
* The Summary page opens successfully.
* The assigned CPU, memory, storage, and network configuration are correct.

---

# Common Issues

| Issue                                | Resolution                                                                              |
| ------------------------------------ | --------------------------------------------------------------------------------------- |
| ISO image is not available           | Upload the required ISO image to ISO storage before creating the VM.                    |
| Insufficient storage                 | Verify that the selected storage has enough free space.                                 |
| Unable to create the virtual machine | Check that the VM ID is not already in use and that sufficient resources are available. |
| Network bridge is unavailable        | Verify that the required Linux Bridge exists and is active.                             |
| Finish button is unavailable         | Review the wizard for incomplete or invalid configuration fields.                       |

---

# Summary

The Create Virtual Machine wizard provides a guided process for deploying new virtual machines in VM2Cloud. By configuring the operating system, storage, CPU, memory, and network settings, administrators can quickly provision virtual machines ready for operating system installation and further configuration.
