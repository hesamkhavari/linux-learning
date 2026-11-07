# Finding Files and System Directories

## 1. Filesystem Hierarchy Standard (FHS)
### The FHS defines the standard directory structure of Linux systems.

Directory|Purpose
|---|---|
`/`|Root of the filesystem hierarchy
`/bin`|Essential user commands
`/sbin`|Essential system administration commands
`/etc`|System-wide configuration files
`/dev`|Device files
`/proc`|Process and kernel information (virtual filesystem)
`/sys`|Kernel, device and hardware information (sysfs)
`/var`|Variable data, logs, caches and mail
`/tmp`|Temporary files
`/usr`|User applications, libraries and documentation
`/home`|Regular users' home directories
`/root`|Root user's home directory
`/boot`|Kernel, initramfs and bootloader files
`/lib`|Essential shared libraries and kernel modules
`/opt`|Optional third-party software
`/mnt`|Temporary manual mount points
`/media`|Removable media mount points
`/srv`|Data provided by system services

### Important Notes :

- `/bin`, `/sbin` and `/lib` are commonly symlinks to corresponding directories under `/usr` on modern Linux distributions.

- `/proc` and `/sys` are virtual filesystems.

- `/tmp` may be cleared during reboot.

- `/var/log` contains system and application logs.

- `/usr/local` is commonly used for locally installed software.

## 2. Finding Files with find
 `find` searches the filesystem directly without relying on an index database.

### Syntax
```bash
find PATH [OPTIONS] [EXPRESSION]
```
### Common Examples
```bash
# Find a file by exact name
find /home -name "hesam.txt"

# Case-insensitive search
find /home -iname "hesam.txt"

# Search by partial filename
find /home -name "*hesam*"

# Find regular files
find /home -type f

# Find directories
find /home -type d

# Find files owned by a user
find /home -user hesam

# Find files modified within the last 7 days
find /home -mtime -7

# Find files larger than 100 MB
find /home -size +100M

# Search the entire filesystem
sudo find / -name "hesam.txt" 2>/dev/null
```

### Useful Options

Option|Description
|---|---|
`-name`|Match filename
`-iname`|Case-insensitive filename match
`-type f`|Regular files
`-type d`|Directories
`-user`|Match file owner
`-group`|Match group owner
`-size`|Match file size
`-mtime`|Match modification time in days
`-perm`|Match permissions
`-empty`|Find empty files or directories
`-maxdepth`|Limit directory traversal depth
`-exec`|Execute a command on matching results

### Execute Commands on Results
```bash
find /home -type f -name "*.log" -exec ls -l {} \;
```

- `{}` represents the current result.

- `\;` terminates the command.


/run

Runtime system data
