# Storage Types

---

## Overview

VM2Cloud supports multiple storage technologies to meet different deployment requirements. Each storage type has its own characteristics, supported content, and recommended use cases.

Choosing the appropriate storage type depends on your infrastructure, performance requirements, scalability, and availability goals.

---

# Directory Storage

## Overview

Directory storage uses a standard directory on the local filesystem to store virtual machine disks, ISO images, container templates, backups, and other supported content.

It is one of the simplest storage types to configure and is commonly used in standalone environments or small deployments.

### Common Use Cases

* Local VM storage
* ISO image repository
* Container templates
* Backup storage
* Lab and development environments

### Advantages

* Simple to configure
* Easy to manage
* Supports multiple content types
* No additional storage technologies required

### Limitations

* Available only on the local node
* Not suitable for High Availability
* No built-in redundancy

---

# LVM Storage

## Overview

LVM (Logical Volume Manager) storage stores virtual machine disks inside logical volumes created from a volume group.

It provides good performance and is commonly used for virtual machine disk storage.

### Common Use Cases

* Virtual machine disks
* Local production storage

### Advantages

* Good performance
* Efficient disk management
* Reliable local storage

### Limitations

* Does not store ISO images or backups
* Local to a single node
* Limited flexibility compared to file-based storage

---

# LVM-Thin Storage

## Overview

LVM-Thin extends LVM by supporting thin provisioning, allowing virtual disks to consume physical space only as data is written.

### Common Use Cases

* Virtual machine disks
* Thin-provisioned environments

### Advantages

* Efficient storage utilization
* Supports snapshots
* Better space management

### Limitations

* Local storage only
* Requires monitoring of available capacity

---

# ZFS Storage

## Overview

ZFS combines a filesystem and volume manager into a single storage platform. It provides advanced features such as snapshots, compression, integrity checking, and RAID configurations.

### Common Use Cases

* Production virtual machines
* High-performance storage
* Data protection

### Advantages

* Data integrity
* Snapshots
* Compression
* RAID support
* High performance

### Limitations

* Requires more memory than other storage types
* Proper planning is recommended before deployment

---

# NFS Storage

## Overview

NFS (Network File System) allows multiple VM2Cloud nodes to access the same shared storage over the network.

It is commonly used in clustered environments.

### Common Use Cases

* Shared VM storage
* ISO repository
* Backup storage
* Cluster deployments

### Advantages

* Shared across multiple nodes
* Easy to configure
* Supports live migration when configured correctly

### Limitations

* Performance depends on the network
* Requires an NFS server

---

# SMB/CIFS Storage

## Overview

SMB/CIFS storage connects VM2Cloud to file shares hosted on Windows or Samba servers.

### Common Use Cases

* Backup storage
* ISO repository
* Shared file storage

### Advantages

* Easy integration with Windows environments
* Supports shared storage

### Limitations

* Performance depends on network quality
* Requires authentication and permissions

---

# Ceph Storage

## Overview

Ceph is a distributed storage platform that provides highly available and scalable storage for clustered environments.

It is recommended for enterprise deployments that require redundancy and high availability.

### Common Use Cases

* High Availability clusters
* Enterprise virtualization
* Distributed storage

### Advantages

* Highly available
* Fault tolerant
* Scalable
* Supports live migration

### Limitations

* Complex to deploy
* Requires multiple nodes
* Higher hardware requirements

---

# Choosing the Right Storage

| Storage Type | Shared | Best For                                          |
| ------------ | ------ | ------------------------------------------------- |
| Directory    | No     | ISO images, backups, templates, small deployments |
| LVM          | No     | Virtual machine disks                             |
| LVM-Thin     | No     | Thin-provisioned virtual machine disks            |
| ZFS          | No     | High-performance local storage                    |
| NFS          | Yes    | Shared cluster storage                            |
| SMB/CIFS     | Yes    | Windows file sharing and backups                  |
| Ceph         | Yes    | Enterprise clusters and High Availability         |

---

# Summary

VM2Cloud supports several storage technologies, each designed for different workloads and deployment scenarios. Selecting the appropriate storage type depends on the intended use, infrastructure design, performance requirements, and availability goals. Understanding the strengths and limitations of each storage type helps administrators build a reliable and efficient virtualization environment.
