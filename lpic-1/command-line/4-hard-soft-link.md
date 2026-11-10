# Hard Links and Symbolic Links

## 1. Inode
An inode stores file metadata, including :

- File type and permissions

- Owner and group

- File size

- Timestamps

- Data block references

- Link count

> The `filename` is stored in the directory entry, not in the inode.

## 2. Hard Links
A `hard link` is another directory entry pointing to the same `inode` as the original file.
```bash
ln TARGET LINK_NAME
```
Example :
```bash
touch hard
ln hard hardlink.txt

ls -li
stat hard
```
### Important Notes :

- Both names share the same inode and file data.

- Changes through either name affect the same file.

- Removing one name does not delete the data while other hard links exist.

- Hard links cannot cross filesystem boundaries.

- Creating hard links to directories is generally prohibited.

## 3. Symbolic Links (Soft Links)
A symbolic link stores the pathname of its target.
```bash
ln -s TARGET LINK_NAME
```
Example :
```bash
touch soft
ln -s soft softlink.txt

ls -li
readlink softlink.txt
readlink -f softlink.txt
```
### Important Notes :

- Has its own inode.

- Can point to files or directories.

- Can reference targets on different filesystems.

- Can become dangling if the target is removed or moved.

- Can use absolute or relative paths.

## 4. Useful Commands

Command|Description
|---|---|
`ln TARGET LINK`|Create a hard link
`ln -s TARGET LINK`|Create a symbolic link
`ls -li`|Display inode numbers
`stat FILE`|Display file metadata
`readlink LINK`|Display the stored link target
`readlink -f LINK`|Resolve the absolute target path
`find DIR -inum INODE`|Find directory entries with a specific inode

## 5. Practical Example
```bash
# Create a hard link
touch original.txt
ln original.txt hardlink.txt

# Create a symbolic link
ln -s original.txt softlink.txt

# Compare inode numbers
ls -li original.txt hardlink.txt softlink.txt

# Remove the original filename
rm original.txt

# Hard link still provides access to the data
cat hardlink.txt

# Symbolic link becomes dangling
cat softlink.txt
```

## 6. Important Comparison

Feature|Hard Link|Symbolic Link
|---|:---:|:---:|
Inode|Shared with target|Own inode
Stores|Directory entry to inode|Target pathname
Cross-filesystem|No|Yes
Directories|Generally prohibited|Supported
Target removed|Data remains if another hard link exists|Becomes dangling
Target renamed|Remains accessible|May break

### Troubleshooting :

- Invalid cross-device link : Hard link source and destination are on `different filesystems`.

- No such file or directory : The symbolic link target may `not exist`.

- Use `df -T PATH` to identify the filesystem containing a path.
