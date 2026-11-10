# Hard Links and Symbolic Links
Linux provides links that allow multiple directory entries to reference the same file or another file.
There are two important types of links:
- Hard Link
- Symbolic Link (Soft Link)
---
## 1. Hard Link
A hard link is another directory entry that refers to the same inode as the original file.
Create a hard link with :
```bash
ln file1 hard1
```
For example:
```bash
touch file1
ln file1 hard1
```
### Now both anmes refer to the same underlying file data.
Check the files :
```bash
ls -li
```
The `-i` option displays the inode number.
Example:
```Plain text
123456 -rw-r--r-- 2 user user 0 file1
123456 -rw-r--r-- 2 user user 0 hard1
```
The important point is that both files have the same inode number.
---
## 2. Modifying a Hard Link
Because both directory entries refer to the same inode, modifying one of them affets the same underlying file.
### For esample:
```bash
echo "Hello Linux" > file1
```
The content can also be read through the hard link:
```bash
cat hard1
```
Output:
```Plain text
Hello Linux
```
---
## 3. Removing a Hard Link
Remove the original filename:
```bash
rm file1
```
The hard link still exists:
```bash
cat hard1
```
The date remains accessible because `hard1` is another directory entry referencing the same inode.
The underlying data is released when the last directory entry referencing it is removed and no process still has the file open.
---
## 4. Symbolic Link
A symbolic link, or symlink, is a special file that stores a path to another file or directory.
### Creatte a symbolic link with:
```bash
ln -s file1 soft1
```
Example:
```bash
touch file1
ln -s file1 soft1
```
Check the result:
```bash
ls -l
```
Example:
```Plain text
-rw-r--r-- 1 user user 0 file1
lrwxrwxrwx 1 user user 5 soft1 -> file1
```
The `->` shows the target of the symbolic link.
---
## 5. Accessing a File Through a Symbolic Link
Write data to the original file:
```bash
echo "Hello Linux" > file1
```
Read it through the symbolic link:
```bash
cat soft1
```
Output:
```Plain text
Hello Linux
```
The symbolic link points to file1.
---
## 6. Removing the Original File
Remove the original file:
```bash
rm file1
```
Now the sumbolic link points to a path that no longer exists.
Check it:
```bash
ls -l
```
The link may appear as:
```Plain text
soft1 -> file1
```
but accessing it will fail because file1 no longer exists.
### This is called a broken symbolic link.
For example:
```bash
cat soft1
```
will produce an error because the target cannot be found.
---
## 7. Hard Link vs Symbolic Link
|Feature|Hard Link|Symbolic Link|
|---:|:---:|:---|
|Command|ln file1 hard1|ln -s file1 soft1|
|Inode|Same as target|Different inode|
|Points to|Same file/inode|A pathname|
|Can reference directories|Normally no|Yes|
|Can cross filesystems|No|Yes|
|Can become broken|No, as long as another link exists|Yes|
|Target deletion|Link still accesses date|Link become broken|
---
## 8. Checking Inodes
Use:
```bash
ls -li
```
Example:
```Plain text
123456 -rw-r--r-- 2 user user 0 file1
123456 -rw-r--r-- 2 user user 0 hard1
123457 lrwxrwxrwx 1 user user 5 soft1 -> file1
```
Here:
- file1 and hard1 have the same inode.
- soft1 has a different inode.
- soft1 stores a reference to the path file1.
---
## 9. Practical Lab
create a test directory:
```bash
mkdir link-lab
cd link-lab
```
Create a file:
```bash
echo "Linux Administration" > file1
```
Create both types of links:
```bash
ln file1 hard1
ln -s file1 soft1
```
Check them:
```bash
ls -li
```
Read through both links:
```bash
cat hard1
cat soft1
```
Remove the original file:
```bash
rm file1
```
Test the links:
```bash
cat hard1
cat soft1
```
The hard link should still work, while the symbolic link should be broken because its target path no longer exists.

clean up:
```bash
rm hard1 soft1
cd ..
rmdir link-lab
```
---
## 10. Important Notes
### Hard Link
A hard link is another name for the same inode and file data.
```bash
ln file1 hard1
```
### Symbolic Link
A symbolic link points to a pathname.
```bash
ln -s file1 soft1
```
### Useful command
Use:
```bash
ls -li
```
to inpect inode numbers and understand the relationship between links.
---
## 11. Real-World Use
Symbolic links are commonly used in Linux systems to provide an alternative path to files or directories.

For example:
```bash
ln -s /path/to/application/current /path/to application/active
```
Applications or administrators can use the stable active path while the actual target can be changed.

This concept is commonly useful when managing software versions, configuration files, and system paths.
