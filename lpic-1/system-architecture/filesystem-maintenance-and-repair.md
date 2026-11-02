# Filesystem Maintenance and Repair

## 1. Core Concepts

- `Inode` : Stores file metadata such as **permissions, ownership, timestamps, and pointers to data blocks**. Filenames are stored in directories and reference inode numbers.

- `Journaling` : Records filesystem metadata changes in a journal to help restore filesystem consistency after an unexpected shutdown. It is not a backup mechanism.

- `Filesystem Check` : Detects and may repair filesystem inconsistencies.

- `Mount / Unmount` : Attaches or detaches a filesystem from the directory tree.

### Inode Allocation

- **ext2/ext3/ext4** : The inode count is generally determined when the filesystem is created.

- **XFS** : Allocates inode structures dynamically as needed.

> A deleted file's inode information may become unavailable or be reused. Recovery is not guaranteed.

## 2. Check Disk Space and Usage
### `df` — Filesystem Usage
```bash
df -h
df -hT
df -i
```
- `df -h` : Shows filesystem capacity in human-readable units.

- `df -hT` : Also displays filesystem types.

- `df -i` : Shows inode usage.

### `du` — Directory and File Usage
```bash
du -h
du -sh ~/Downloads/
du -h --max-depth=1 /var
du -ah /var
```
- `du -h` : Displays disk usage in human-readable units.

- `du -sh` : Shows the total size of a directory.

- `du -h --max-depth=1` : Shows usage of immediate subdirectories.

- `du -ah` : Includes individual files.

> Difference: `df` reports filesystem-level usage, while `du` calculates usage from files and directories.

## 3. Mount and Unmount Filesystems
```bash
findmnt
mount
lsblk -f
```
Mount an ext4 filesystem :
```bash
sudo mount -t ext4 /dev/sdb1 /mnt/test
```
Unmount it :
```bash
sudo umount /mnt/test
```
Check whether a filesystem is mounted :
```bash
findmnt /mnt/test
```
> A filesystem should generally be `unmounted` before running repair tools.

## 4. Filesystem Checking and Repair
### General fsck Command
`fsck` is a frontend for filesystem-specific checkers.
```bash
sudo fsck /dev/sdb1
sudo fsck -t ext4 /dev/sdb1
```
For ext filesystems, the dedicated checker is :
```bash
sudo e2fsck -f /dev/sdb1
```

- `-f` : Forces a filesystem check.

- Run checks on unmounted filesystems.

- Review the results before accepting repairs.

### XFS Repair
Use `xfs_repair` for XFS filesystems :
```bash
sudo xfs_repair -n /dev/sdb5
sudo xfs_repair /dev/sdb5
```

- `-n` : Checks for problems without modifying the filesystem.

- `Without -n` : Attempts repairs.

### Important:
- xfs_check is obsolete; use `xfs_repair`.

- XFS filesystems must be `unmounted` before repair.

- `xfs_repair -L` clears the log and can cause data loss. Use it only as a last resort after understanding the consequences.

### Root Filesystem Repair
A root filesystem normally cannot be safely checked or repaired while it is mounted and actively in use.

Use a `Live Linux` environment or Rescue Mode to check and repair it offline.

## 5. ext Filesystem Utilities
`dumpe2fs` — Display ext Filesystem Information
```bash
sudo dumpe2fs -h /dev/sdb1
sudo dumpe2fs /dev/sdb1
```
Displays filesystem information such as block size, inode count, features, and superblock details.

### `tune2fs` — Modify ext Filesystem Parameters
```bash
sudo tune2fs -l /dev/sdb1
```
Displays filesystem parameters and settings.

`tune2fs` can modify certain **ext** filesystem features and parameters. 
For example, journal-related features can be changed, but enabling or disabling journaling should not be done casually. 
It requires an unmounted filesystem, a backup, and appropriate consistency checks.

### `debugfs` — Inspect ext Filesystems
```bash
sudo debugfs /dev/sdb1
```
Useful commands inside debugfs :

Command|Purpose
|---|---|
`ls`|List directory contents
`stat <inode>`|Display inode information
`lsdel`|List recently deleted inodes, when available
`dump <inode> file.txt`|Extract an inode's contents
`quit`|Exit

`debugfs` is primarily a low-level inspection and debugging tool. It can also modify filesystem structures in write mode.

Deleted-file recovery using `inode` information is not guaranteed. 
Avoid writing to the filesystem after deletion if recovery is important. Work on a disk image or copy whenever possible.

## 6. XFS Utilities
### xfs_info — Display XFS Information
```bash
sudo xfs_info /mount/point
```
Displays XFS geometry and filesystem configuration.

### xfs_metadump — Capture Metadata for Analysis
```bash
sudo xfs_metadump /dev/sdb5 xfs-metadata.dump
```
Creates a metadata dump that can help with offline troubleshooting and analysis. It does not create a full data backup.

## 7. Important Operational Notes

- Always identify the correct device using `lsblk -f` before running repair commands.

- Unmount the filesystem before running `e2fsck` or `xfs_repair`.

- Prefer a non-modifying check first when the tool supports it.

- Never treat filesystem repair as a replacement for backups.

- Avoid formatting or modifying a filesystem while it contains important data.

- For damaged filesystems, create a disk image or backup before attempting recovery whenever possible.

### Quick Reference

Task|Command
|---|---|
Check filesystem usage|`df -hT`
Check inode usage|`df -i`
Show directory size|`du -sh /path`
Show subdirectory sizes|`du -h --max-depth=1 /path`
List mounted filesystems|`findmnt`
Check ext filesystem|`sudo e2fsck -f /dev/sdXN`
Check XFS without modifying|`sudo xfs_repair -n /dev/sdXN`
Repair XFS|`sudo xfs_repair /dev/sdXN`
Display ext information|`sudo dumpe2fs -h /dev/sdXN`
Display ext settings|`sudo tune2fs -l /dev/sdXN`
Display XFS information|`sudo xfs_info /mount/point`
Inspect ext filesystem|`sudo debugfs /dev/sdXN`
Display block devices|`lsblk -f`
