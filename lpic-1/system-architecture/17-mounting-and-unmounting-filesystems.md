# Mounting and Unmounting Filesystems

## 1. Core Concepts
- **Mount** : Attaches a filesystem to a directory (mount point).

- **Unmount** : Detaches a mounted filesystem.

- **Mount Point** : An existing directory used to access a filesystem.

- A mount point should normally be an empty directory.

> On systems without a desktop environment, removable devices usually need to be mounted manually unless an automount service is configured.

## 2. Create a Mount Point
```bash
sudo mkdir -p /mnt/external
```

## 3. Mount a Filesystem
General syntax :
```bash
sudo mount -t TYPE DEVICE MOUNT_POINT
```
Example :
```bash
sudo mount -t ext3 /dev/sdb1 /mnt/external
```
Useful commands :
```bash
mount
findmnt
findmnt /mnt/external
lsblk -f
```

- `mount` : Lists currently mounted filesystems.

- `findmnt` : Displays mounted filesystems and their relationships.

- `lsblk -f` : Shows devices, filesystem types, labels, and UUIDs.

### Mount with Options
Read-only mount :
```bash
sudo mount -o ro /dev/sdb1 /mnt/external
```
Read-write mount :
```bash
sudo mount -o rw /dev/sdb1 /mnt/external
```
Mount with a specific filesystem type :
```bash
sudo mount -t ext4 /dev/sdb1 /mnt/external
```

## 4. Unmount a Filesystem
Unmount by mount point :
```bash
sudo umount /mnt/external
```
Or by device :
```bash
sudo umount /dev/sdb1
```
If the filesystem is busy, identify processes using it :
```bash
sudo fuser -vm /mnt/external
```
Close applications or leave the directory before unmounting.

> Important : Do not use forced or lazy unmount options as a routine solution to a busy filesystem.

## 5. Persistent Mounting with /etc/fstab
The `/etc/fstab` file defines filesystems that should be **mounted automatically** or made available through system mount operations.

Example entry :
```bash
UUID=<filesystem-uuid> /mnt/external ext4 defaults,nofail 0 2
```

### fstab Fields

Field|Meaning
|---|---|
1|Device, UUID, or LABEL
2|Mount point
3|Filesystem type
4|Mount options
5|Dump setting
6|Filesystem check order

Find the UUID :
```bash
sudo blkid /dev/sdb1
```

### Common Mount Options

Option|Purpose
|---|---|
defaults|Uses standard mount options
ro|Read-only
rw|Read-write
auto|Included in `mount -a`
noauto|Not mounted by `mount -a`
user|Allows an ordinary user to `mount` the filesystem
users|Allows any user to `unmount` a user-mounted filesystem
noexec|Prevents execution of binaries from the filesystem
nosuid|Ignores **set-user-ID** and **set-group-ID** bits
nodev|Does not interpret device files on the filesystem
nofail|Does not make boot fail if the device is unavailable

### Validate fstab
After editing `/etc/fstab`, test it :
```bash
sudo findmnt --verify
sudo mount -a
findmnt /mnt/external
```
Important: Incorrect `/etc/fstab` entries can cause boot problems. Back up the file before editing it.

## 6. Useful Mount Commands
Mount all eligible filesystems listed in `/etc/fstab` :
```bash
sudo mount -a
```
Display filesystem information :
```bash
findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS
```
Display filesystem usage :
```bash
df -hT
```
Unmount all eligible filesystems :
```bash
sudo umount -a
```

> Use `umount -a` cautiously. It can disconnect multiple filesystems and is not recommended for routine administration.

### Quick Reference

Task|Command
|---|---|
Create mount point|`sudo mkdir -p /mnt/external`
Mount filesystem|`sudo mount /dev/sdb1 /mnt/external`
Mount read-only|`sudo mount -o ro /dev/sdb1 /mnt/external`
List mounted filesystems|`findmnt`
Unmount filesystem|`sudo umount /mnt/external`
Find filesystem UUID|`sudo blkid /dev/sdb1`
Verify fstab|`sudo findmnt --verify`
Mount fstab entries|`sudo mount -a`
Find processes using mount point|`sudo fuser -vm /mnt/external`
Check filesystem usage|`df -hT`
