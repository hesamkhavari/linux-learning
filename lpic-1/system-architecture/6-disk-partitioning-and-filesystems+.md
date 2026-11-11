# Disk Partitioning, Filesystems, and Swap

## 1. Core Concepts

- Partition: A logical division of a physical disk.

- Partition Table: Stores partition layout information. The two common formats are MBR and GPT.

- Filesystem: Organizes files and directories on a partition or logical volume.

- Swap: Disk space used by Linux as backing storage for memory pages when needed.

### Partition Type vs. Filesystem Type

Concept|Tool|Purpose
|---|:---:|---|
Partition type|`fdisk`|Defines the partition's type identifier in the partition table
Filesystem creation|`mkfs`|Creates filesystem structures on a partition
Swap initialization|`mkswap`|Prepares a partition or file for use as swap
Swap activation|`swapon`|Enables swap for the running system

> Important: Setting a partition type does not create a filesystem or initialize swap. These are separate operations.

## 2. MBR vs. GPT

- MBR : Supports up to four partition-table entries. An extended partition can be used to contain logical partitions.

- GPT : Does not use the MBR primary/extended/logical partition scheme and is generally preferred for modern systems.

- Under the traditional MBR layout, logical partitions are commonly numbered from 5 onward.

## 3. Inspect Disks and Partitions

```bash
lsblk
lsblk -f
sudo fdisk -l
sudo fdisk -l /dev/sdb
sudo blkid
```

- `lsblk` : Lists block devices and their relationships.

- `lsblk -f` : Shows filesystem types, labels, and UUIDs.

- `fdisk -l` : Displays partition tables.

- `blkid` : Displays filesystem signatures and identifiers.

> Always verify the target device before modifying it. Device names such as `/dev/sdb` can change between boots or systems.

## 4. Partition Management with fdisk

Open the partition editor for a disk :
```bash
sudo fdisk /dev/sdb
```

Common interactive commands :

Command|Purpose
|---|---|
p|Print the partition table
n|Create a partition
d|Delete a partition
t|Change a partition type
w|Write changes and exit
q|Exit without writing changes

The available partition options depend on whether the disk uses `MBR` or `GPT`.

### Common MBR Partition Type IDs

ID|Meaning
|---|---|
83|Linux filesystem
82|Linux swap
8e|Linux LVM
07|HPFS/NTFS/exFAT type
0c|FAT32 with LBA

**Note** : MBR type IDs are metadata; they do not format a partition. For example, selecting Linux swap does not replace the need to run mkswap.

## 5. Create Filesystems

General syntax :
```bash
sudo mkfs -t TYPE /dev/PARTITION
```
Examples for a dedicated test disk :
```bash
sudo mkfs.ext2 /dev/sdb5
sudo mkfs.ext3 /dev/sdb6
sudo mkfs.ext4 /dev/sdb7
sudo mkfs.xfs /dev/sdb1
sudo mkfs.vfat -F 32 /dev/sdb3
```
For `ReiserFS`, if the required utilities are installed:
```bash
sudo mkfs.reiserfs /dev/sdbX
```
For `NTFS`, if the appropriate utility is installed:
```bash
sudo mkfs.ntfs /dev/sdbX
```
Replace `/dev/sdbX` with the intended partition. 
These commands can overwrite existing filesystem structures and make existing data inaccessible. Run them only on the correct test partition.

### Filesystem Overview

- `ext2` : An older Linux filesystem **without journaling**.

- `ext3` : An older ext filesystem with **journaling**.

- `ext4` : A widely used general-purpose Linux filesystem.

- `XFS` : A mature filesystem commonly used for **large files** and **high-throughput** workloads.

- `VFAT/FAT32` : Useful when broad **compatibility** with other operating systems and devices is required.

- `NTFS` : The primary **Windows filesystem**; Linux creation and management depend on installed tools.

- `ReiserFS` : A **legacy** filesystem and generally not a first choice for new deployments.

## 6. Create and Enable Swap
The following example assumes `/dev/sdb2` is a dedicated test partition:
```bash
sudo mkswap /dev/sdb2
sudo swapon /dev/sdb2
swapon --show
cat /proc/swaps
```

- `mkswap` : Initializes the selected area for swap use.

- `swapon` : Enables swap.

- `swapon --show` : Lists active swap areas.

- `/proc/swaps` : Provides information about currently enabled swap areas.

To disable a swap partition :
```bash
sudo swapoff /dev/sdb2
```
> Ensure the system has sufficient available memory before disabling swap that is in use.

### Enable Swap at Boot
Find the swap UUID :
```bash
sudo blkid /dev/sdb2
```
Add an entry to `/etc/fstab`, replacing the placeholder with the actual UUID :
```bash
UUID=<swap-uuid> none swap sw 0 0
```
> Check for duplicate entries before editing the file. Validate the configuration before rebooting.

## 7. Mount and Inspect a Filesystem
Mount a test filesystem :
```bash
sudo mkdir -p /mnt/test
sudo mount /dev/sdb7 /mnt/test
findmnt /mnt/test
df -hT /mnt/test
sudo umount /mnt/test
```

- `mount` : Attaches a filesystem to a directory.

- `findmnt` : Shows mounted filesystems and mount options.

- `df -hT` : Displays filesystem type and disk usage in human-readable units.

- `umount` : Detaches a mounted filesystem.

> Use the correct device and mount point. A filesystem must not be unmounted while it is still needed by active processes.

## 8. Useful Operational Notes

- Formatting and partition-table changes can cause data loss. `Back up` important data first.

- Verify device names with `lsblk` immediately before destructive operations.

- `sudo partprobe /dev/sdb` can ask the kernel to reread a disk's partition table. It may not work if the disk or partitions are busy.

- Use UUIDs in `/etc/fstab` when appropriate, rather than relying on potentially changing device names.

- `mkfs` utilities for filesystems such as `XFS`, `NTFS`, and `ReiserFS` may need to be installed separately.

- For modern Linux systems, `ext4` and `XFS` are common filesystem choices; select based on workload and operational requirements.

## Quick Reference

Task|Command
|---|---|
List block devices|lsblk -f
List partition tables|sudo fdisk -l
Edit a partition table|sudo fdisk /dev/sdb
Create an ext4 filesystem|sudo mkfs.ext4 /dev/sdXN
Initialize swap|sudo mkswap /dev/sdXN
Enable swap|sudo swapon /dev/sdXN
Show active swap|swapon --show
Show filesystem UUIDs|sudo blkid
Check filesystem usage|df -hT
Inspect mount points|findmnt

