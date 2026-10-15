# Disk Partitioning and Filesystems

## 1. Core Concepts
- **Partition** : A logical division of a disk.
- **Filesystem** : Organizes files and directories, e.g. ext4, XFS.
- **Mount Point** : Directory where a filesystem becomes accessible.
- **Swap** : Disk space used as virtual memory; it can be a partition or file.
- **Network Filesystem** : Allows access to remote storage, e.g. NFS.

## 2. Common Directories
Path|Purpose
|:---|:---:|
/|Root filesystem
/boot|Kernel and boot-related files
/boot/efi|Common EFI System Partition mount point on UEFI systems
/home|Users' personal directories
/var|Variable data, logs, caches and service data
/root|Root user's home directory
Swap|Virtual memory space; not a directory

## 3. Storage Design
- **Simple desktop** : One `/` filesystem, required boot partition(s), and optional `swap`.

- **Networked desktop** : `/home` may use a network filesystem such as NFS to make user files accessible across multiple systems.

- **Server**: Separate filesystems for `/var`, `/home`, or other paths may help with capacity management, isolation and maintenance. Design depends on workload and requirements.

## 4. RAID Basics
- **Hardware RAID** : Managed by a dedicated controller.
- **Software RAID** : Managed by the operating system, commonly using Linux md.
- **RAID 0** : Striping for performance and capacity, with no redundancy. Failure of one member can make the entire array unavailable.
- **RAID 1** : Mirroring, providing redundancy.

> `/boot` does not always need to be outside RAID. Boot support depends on the bootloader, RAID implementation, filesystem and system configuration.

- **Important** : RAID is not a substitute for backups.

## 5. Useful Commands
List disks and partition tables:
```bash
sudo fdisk -l
```
Show block devices, filesystems and mount points:
```bash
lsblk -f
```
Show mounted filesystems and usage:
```bash
df -hT
```
Show current mounts:
```bash
findmnt
```
