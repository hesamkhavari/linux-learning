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

