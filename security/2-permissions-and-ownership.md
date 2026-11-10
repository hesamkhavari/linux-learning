# Linux Permissions and Ownership
### Linux file access is controlled through ownership and permission bits.
## Permission Classes
Three classes exist:
```Plain text
u = user/owner
g = group
o = others
a = all
```
Basic permissions:
```Plain text
r = read
w = write
x = execute
```
For directories, `x` means the ability to access/search the directory.

## chmod
Change permissions:
```bash
chmod 764 file1.txt
```
Permission values:
```Plain text
r = 4
w = 2
x = 1
```
Therefore:
```Plain text
7 = rwx
6 = rw-
4 = r--
```
Symbolic mode:
```bash
chmod ugo+rwx file.txt
chmod a-rwx file.txt
chmod g-x file.txt
chmod o-wx file.txt
```
Multiple changes:
```bash
chmod a+rwx,g-x,o-wx file.txt
```
Recursive change:
```bash
chmod -R 755 directory/
```
> Be extremely careful with recursive permission changes.
`chmod` supports both symbolic and octal modes.

## chown
Change file owner:
```bash
sudo chown user1 file1.txt
```
Change owner and group:
```bash
sudo chown user2:adm file2.txt
```
Change only group:
```bash
sudo chown :adm file1.txt
```
Recursive ownership change:
```bash
sudo chown -R user1:adm directory/
```
`chown` can change owner and/or group ownership.

## chgrp
Change group ownership:
```bash
sudo chgrp adm file1.txt
```

## Special Permission Bits
### Linux has three important special permission bits.
|Bit	|Octal	|Purpose|
|:---|:---:|:---:|
|SUID	|4000	|Execute file with owner's effective privileges|
|SGID	|2000	|Execute with group's effective privileges / inherit group on directories|
|Sticky	|1000	|Restrict deletion in shared directories|

## SUID
Find SUID files:
```bash
sudo find / -type f -perm /4000 2>/dev/null
```
Example:
```bash
ls -l /usr/bin/passwd
```
A SUID executable may show:
```Plain text
-rwsr-xr-x
```
Set SUID:
```bash
chmod u+s file
```
or:
```bash
chmod 4755 file
```
> Never enable SUID on arbitrary programs. It can create serious privilege-escalation risks.

## SGID
Find SGID files:
```bash
sudo find / -type f -perm /2000 2>/dev/null
```
Set SGID:
```bash
chmod g+s file
```
or:
```bash
chmod 2755 file
```
### For directories, SGID causes newly created files and subdirectories to inherit the directory's group.
Example:
```bash
chmod 2775 shared/
```

## Sticky Bit
Find directories with Sticky Bit:
```bash
sudo find / -type d -perm -1000 2>/dev/null
```
Set Sticky Bit:
```bash
chmod +t directory/
```
or:
```bash
chmod 1777 directory/
```
Typical example:
```bash
ls -ld /tmp
```
A sticky directory commonly appears as:
```Plain text
drwxrwxrwt
```
The Sticky Bit prevents ordinary users from deleting or renaming files belonging to other users in a shared writable directory.

## Special File Types
Linux supports different file types.
Check with:
```bash
ls -l
```
Examples:
```Plain text
- regular file
d directory
l symbolic link
p named pipe (FIFO)
s socket
c character device
b block device
```
Create a FIFO:
```bash
mkfifo pipe1
```
Inspect device files:
```bash
ls -l /dev/null 
ls -l /dev/sr0
```

## umask
Display current umask:
```bash
umask
```
Symbolic form:
```bash
umask -S
```
Set a restrictive umask:
```bash
umask 077
```
Then test:
```bash
touch private.txt
ls -l private.txt
```
`umask` removes permission bits from the permissions requested when new files/directories are created. It does not directly change permissions on existing files.
> `umask 0777` is technically valid but is generally a demonstration rather than a practical configuration. It would remove all normal permission bits from newly created objects.
>
> ## Practical Permission Model
> For a normal file:
> ```Plain text
> -rw-r--r--
> ```
> means:
> ```Plain text
> owner → rw-
> group → r--
> others → r--
> ```
> For a directory:
> ```Plain text
> drwxr-x---
> ```
> means:
> ```Plain text
> owner → rwx
> group → r-x
> others → ---
> ```
> Remember that directory permissions have different practical meanings from regular files.
