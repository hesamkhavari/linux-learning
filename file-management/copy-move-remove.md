# Copy, Move, and Remove Files and Directories
This document covers the basic Linux commands used to copy, move, rename, and remove files and directories.
---
## 1. cp
The `cp` command is used to copy files and directories.
### Basic syntax
```bash
cp [SOURCE] [DESTINATION]
```
### Copy a file
```bash
cp file1 copy1
```
This creates a copy of file1 named copy1.
### Copy a file into a directory
```bash
cp file1 folder1/
```
The file is copied into folder1.
### Copy a directory
To copy a directory and its contents, use `-R` or `-r`.
```bash
cp -r folder1 folder2/
```
This copies folder1 and its contents into folder2.
## Useful options
### Preserve attributes
```bash
cp -p file1 copy1
```
The `-p` option preserves important file attributes such as permissions, ownership, and timestamps when possible.
### Interactive mode
```bash
cp -i file1 folder1/
```
The `-i` option asks for confirmation before overwriting an existing file.
### Update mode
```bash
cp -u file1 folder1/
```
The `-u` option copies the source file only when it is newer than the destination or the destination does not exist.
### Force mode
```bash
cp -f file1 folder1/
```
The `-f` option forces the copy operation when possible.
---
## 2. mv
The `mv` command is used to move or rename files and directories.
### Basic syntax
```bash
mv [SOURCE] [DESTINATION]
```
### Rename a file
```bash
mv file1 file2
```
if file2 does not already exist, this effectively renames file1 to file2.
### Move a file into a directory
```bash
mv file1 folder1/
```
The file is moved into folder1.
### Move a directory
```bash
mv folder1 /home/
```
The directory is moved to /home/.
### Rename a directory
```bash
mv folder1 folder2
```
This renames folder1 to folder2.
---
## 3. rm
The `rm` command is used to remove files and directories.
### Remove a file
```bash
rm file1
```
### Interactive removal
```bash
rm -i file1
```
The `-i` option asks for confirmation before removing the file.
### Remove a directory and its contents
```bash
rm -r folder1
```
The `-r` option means recursive removal.
### Force removal
```bash
rm -f file1
```
The `-f` option forces removal and suppresses some confirmation/error messages.
### Remove a directory recursively and forcefully
```bash
rm -rf folder1
```
This recursively removes the directory and its contents whithout interactive confirmation.
---
## 4. rmdir
The rmdir command removes empty directories.
```bash
rmdir folder1
```
If the directory contains files or subdirectories, `rmdir` will normally fail.

This makes `rmdir` different from:
```bash
rm -r folder1
```
because `rm -r` can remove a directory and its contents.
---
## 5. Important differences
cp        ---> Copy files/directories

mv        ---> Move or rename files/directories

rm        ---> Remove files/directories

rmdir     ---> Remove empty directories
---
## 6. Practical example
### Create a test environment:
```bash
mkdir lab
cd lab

touch file1
mkdir folder1
```
### Copy the file:
```bash
cp file1 folder1/
```
### Rename the file:
```bash
mv file1 file2
```
### Move the renamed file:
```bash
mv file2 folder1/
```
### Check the directory:
```bash
ls -la folder1
```
### Remove the file:
```bash
rm folder1/file2
```
### Remove the empty directory:
```bash
rmdir folder1
```
### Return to the parent dierctory:
```bash
cd ..
```
### Remove the test directory:
```bash
rmdir lab
```
---
## 7. Important Notes
- `cp` creates a copy; it does not remove the orginal.
- `mv` can be used both for moving and renaming.
- `rmdir` only removes empty directories.
- `rm -r` recursively removes directories and their contents.
- `rm -rf` is a powerful and potentailly destructive command. Always verify the path before executing it.
- Be especially careful when using `rm` as root.
- Before using destructive commands, check the current directory with:
```bash
pwd
```
and inspact the target with:
```bash
ls -la
```
---
## 8. Safety Rule
Never run a destructive command without first verifying:
1. Where you are :
```bash
pwd
```
2. What you are about to remove:
```bash
ls -la
```
3. The exact target path.
For learning and testing, use a dedicated lab directory rather than important system directories.
