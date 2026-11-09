# Disk Quotas

## 1. Core Concepts
Disk Quotas limit how much disk space and how many inodes users or groups can consume on a filesystem.

### Two types of limits are available :

Limit|Description
|---|---|
Block Quota|Limits disk space usage
Inode Quota|Limits the number of files and directories

### Soft Limit vs. Hard Limit

- `Soft Limit` : Can be exceeded temporarily during the configured grace period.

- `Hard Limit` : Maximum allowed usage; cannot be exceeded.

- `Grace Period` : Time allowed to remain above the soft limit before further usage is restricted.

> The grace period is configurable and is not necessarily 7 days.


Note: Quota `block` values are generally displayed in `KiB`. `Inode` limits control the `number of filesystem` objects, not their size.

## 2. Install Quota Tools
Debian / Ubuntu :
```bash
sudo apt update
sudo apt install quota
```
RHEL-based systems :
```bash
sudo dnf install quota
```

## 3. Enable Quotas in `/etc/fstab`
Example entry using UUID :
```bash
UUID=<filesystem-uuid> /home/quota ext4 defaults,usrquota,grpquota 0 2
```

- `usrquota` : Enables user quotas.

- `grpquota` : Enables group quotas.

Create the mount point if needed :
```bash
sudo mkdir -p /home/quota
```
Apply the mount configuration :
```bash
sudo mount -o remount /home/quota
```
Check mount options :
```bash
findmnt -no OPTIONS /home/quota
```

## 4. Initialize Quota Database
Run a filesystem quota check :
```bash
sudo quotacheck -cug /home/quota
```
- `-c` : Creates quota database files.

- `-u` : Checks user quotas.

- `-g` : Checks group quotas.

For reliable results, run `quotacheck` with the filesystem `unmounted` or in an appropriate read-only state. 
Avoid running it against a filesystem that is actively changing.

## 5. Enable Quotas
```bash
sudo quotaon /home/quota
```
Enable quotas on all configured filesystems :
```bash
sudo quotaon -a
```
Disable quotas :
```bash
sudo quotaoff /home/quota
```

## 6. Set User Quotas
Edit a user's quota :
```bash
sudo edquota -u hesam
```
The editor displays fields similar to :
```bash
Filesystem      blocks   soft   hard   inodes   soft   hard
/dev/sdb1          16    1100   1500       5      0      0
```

Field|Meaning
|---|---|
blocks|Current disk usage
block soft|Soft disk-space limit
block hard|Hard disk-space limit
inodes|Current inode usage
inode soft|Soft file-count limit
inode hard|Hard file-count limit

In this example :

- Block soft limit : 1100 KiB

- Block hard limit : 1500 KiB

- Inode soft and hard limits : Disabled (0)

### Alternative: `setquota`
Set limits directly from the command line:
```bash
sudo setquota -u hesam 1100 1500 5 10 /home/quota
```
Syntax :
```bash
setquota -u USER BLOCK_SOFT BLOCK_HARD INODE_SOFT INODE_HARD MOUNT_POINT
```

## 7. Configure Grace Period
Edit grace-period settings :
```bash
sudo edquota -t
```
This allows you to configure the time users can remain above their soft limits.

## 8. Monitor Quota Usage
Check your own quota :
```bash
quota -s
```
Check a specific user :
```bash
sudo quota -u hesam
```
Generate a quota report :
```bash
sudo repquota -a
```
Report quotas for a specific filesystem :
```bash
sudo repquota /home/quota
```

- `quota` : Displays quota usage and limits.

- `repquota` : Generates a report for users and groups.

## 9. Important Notes

- Quotas are configured per `filesystem`, not simply per directory.

- `User quotas` limit the total usage of a user on that filesystem.

- `Group quotas` limit usage associated with a group.

- Quota limits do not replace filesystem permissions.

- A user can still be denied access by `permissions` even when quota space is available.

### Quick Reference

Task|Command
|---|---|
Install quota tools|`sudo apt install quota`
Enable user/group quotas|`usrquota,grpquota in /etc/fstab`
Initialize quota database|`sudo quotacheck -cug /mountpoint`
Enable quotas|`sudo quotaon /mountpoint`
Disable quotas|`sudo quotaoff /mountpoint`
Edit user quota|`sudo edquota -u USER`
Edit group quota|`sudo edquota -g GROUP`
Set quota directly|`sudo setquota ...`
Configure grace period|`sudo edquota -t`
Check user quota|`quota -s`
Generate quota report|`sudo repquota -a`
