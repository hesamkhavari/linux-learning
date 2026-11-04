# Linux Permissions and Ownership

## 1. File Types and Permissions
Display file permissions :
```bash
ls -l
ls -la
```
Common file types :

Symbol|Type
|---|---|
`-`|Regular file
`d`|Directory
`l`|Symbolic link

Example :
```bash
-rwxr-xr-- 1 hesam users 1024 file.txt
```
Permission groups :

- First three characters: `Owner (u)`

- Next three characters: `Group (g)`

- Last three characters: `Others (o)`

## 2. Permission Values

Permission|Symbol|Numeric Value
|---|:---:|---|
Read|r|4
Write|w|2
Execute|x|1

Common permission combinations :

Numeric|Permission
|---|---|
777|rwxrwxrwx
755|rwxr-xr-x
644|rw-r--r--
700|rwx------
600|rw-------
666|rw-rw-rw-

> Note : Directories require execute (`x`) permission to access or traverse their contents.

## 3. Change Permissions with chmod
### Symbolic Mode
```bash
chmod u+x script.sh
chmod g-w file.txt
chmod o-r file.txt
chmod a+r file.txt
chmod u=rwx,g=rx,o= file.txt
```

- `u` : Owner

- `g` : Group

- `o` : Others

- `a` : All users

- `+` : Add permission

- `-` : Remove permission

- `=` : Set exact permission

### Numeric Mode
```bash
chmod 755 script.sh
chmod 644 file.txt
chmod 600 private.txt
chmod 700 private_directory
```
Recursive permission changes :
```bash
chmod -R 755 directory/
```
Use `recursive` changes carefully. 
Applying the same permissions to files and directories can create security or functionality problems.

## 4. Change Ownership with chown and chgrp
Change file owner :
```bash
sudo chown root file.txt
```
Change group :
```bash
sudo chgrp root file.txt
```
Change owner and group together :
```bash
sudo chown root:root file.txt
```
Change only the group :
```bash
sudo chown :hesam file.txt
```
Recursive ownership change :
```bash
sudo chown -R root:root directory/
```
Check ownership :
```bash
ls -l
stat file.txt
```

## 5. Default Permissions and umask

`umask` determines which permission bits are removed from the default permissions when new files and directories are created.

Check the current value :
```bash
umask
umask -S
```
Typical initial permissions before applying umask :

- Files: 666

- Directories: 777

Example :
```bash
umask 022
```
Results :

Object|Result
|---|---|
New file|644
New directory|755

Another example :
```bash
umask 077
```

Results :

Object|Result
|---|---|
New file|600
New directory|700

### Configure umask
For the current shell session :
```bash
umask 027
```
For a user login shell, a common location is :
```bash
~/.profile
```
Add :
```bash
umask 027
```

System-wide configuration may be managed through
`/etc/profile`, `/etc/profile.d/`, or distribution-specific login configuration.

> Important: `umask` removes permission bits;
> it does not grant permissions. The resulting permissions also depend on the application creating the file.

## 6. Special Permissions
Linux provides three special permission bits :

Permission|Numeric Value|Purpose
|---|:---:|---|
SUID|4|Runs an executable with the file `owner's effective privileges`
SGID|2|Runs an executable with the file `group's effective privileges`; on directories, new items inherit the directory's group
Sticky Bit|1|Restricts `deletion` and `renaming` in shared writable directories

### SUID — Set User ID
```bash
sudo chmod u+s script.sh
sudo chmod 4755 script.sh
```

- When an executable has SUID set, it runs with the effective user ID of its owner.

- It is commonly used by specific system utilities.

- SUID on scripts is ignored on many Linux systems.

Permission display :

- `s` : `Execute` permission is also set.

- `S` : `Execute` permission is not set.

### SGID — Set Group ID
```bash
sudo chmod g+s script.sh
sudo chmod 2755 directory/
```

> On a directory, SGID causes newly created files and subdirectories to `inherit` the directory's group
> in typical Linux configurations.

Permission display :
- `s` : Group execute permission is set.

- `S` : Group execute permission is not set.

### Sticky Bit
```bash
sudo chmod +t shared/
sudo chmod 1777 shared/
```
Common example :
```bash
ls -ld /tmp
```
> The `/tmp` directory commonly uses permissions `1777`.



In a `sticky directory`, users generally cannot `delete` or `rename` files owned by other users, 
even when the directory is writable by everyone. Root and the directory owner have additional privileges.

Permission display :
- `t` : Others execute permission is set.

- `T` : Others execute permission is not set.

## 7. Symbolic Links
Create a symbolic link :
```bash
ln -s /path/to/file link-name
```
Inspect it :
```bash
ls -l link-name
```

Symbolic links commonly display `777` permissions, 
but access is governed by the target's permissions and directory traversal permissions.

## 8. Useful Security Commands
Find files with SUID or SGID permissions :
```bash
sudo find / -type f -perm /6000 -ls 2>/dev/null
```
Find world-writable directories :
```bash
sudo find / -type d -perm -0002 -ls 2>/dev/null
```
Check effective user and group IDs :
```bash
id
```
Check file metadata :
```bash
stat file.txt
```

## Quick Reference

Task|Command
|---|---|
List permissions|ls -l
Change permissions|chmod 755 file
Add execute permission|chmod u+x file
Change owner|chown user file
Change group|chgrp group file
Change owner and group|chown user:group file
Check current umask|umask
Set umask|umask 027
Set SUID|chmod u+s file
Set SGID|chmod g+s directory
Set Sticky Bit|chmod +t directory
Inspect metadata|stat file
Find SUID/SGID files|find / -type f -perm /6000
Check user identity|id
