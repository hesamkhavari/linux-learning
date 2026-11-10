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
`/run`Runtime system data

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

## 3. Finding Files with locate
`locate` searches a previously generated database of file paths.

```bash
# Search for a filename
locate hesam

# Case-insensitive search
locate -i hesam

# Search for an exact basename
locate -b '\hesam.txt'

# Update the database
sudo updatedb
```

### Important Notes :
- `locate` is generally faster than `find`.

- Results depend on database freshness.

- Newly created files may not appear until the database is updated.

- The database configuration is commonly stored in `/etc/updatedb.conf`.

- locate supports patterns and is not limited to exact filename matching.

## 4. Finding Commands and System Information

Command|Purpose
|---|---|
`which` COMMAND|Locate an executable in PATH
`whereis` COMMAND|Locate binary, source and manual files
`whatis` COMMAND|Display a short manual description
`type` COMMAND|Identify command type
`command -v` COMMAND|Identify how the shell resolves a command
`man` hier|Display filesystem hierarchy documentation

### Examples
```bash
which ls
whereis ls
whatis ls
type ls
command -v ls
```

Important: `type` can identify aliases, shell functions, built-ins and external commands. 
`command -v` is generally preferable for portable shell scripting.

## 5. Quick Reference

Requirement|Location / Command
|---|---|
System configuration|`/etc`
System logs|`/var/log`
Mail data|`/var/mail` or `/var/spool/mail`
Device files|`/dev`
Process information|`/proc`
Hardware and kernel device information|`/sys`
Temporary files|`/tmp`
User home directories|`/home`
Root user's home|`/root`
Boot files|`/boot`
Manual file search|`find`
Database-based search|`locate`
Update locate database|`updatedb`

## 6. Important Notes

- `find` performs a live filesystem search and does not require a database.

- `locate` searches an index and may return outdated results.

- Use `sudo` when permission restrictions prevent access to directories.

- Use `2>/dev/null` to suppress permission error messages.

- Use `find` when you need conditions such as ownership, permissions, size or modification time.
