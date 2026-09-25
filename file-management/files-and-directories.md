# Files and Directories
This section covers basic Linux commands for navigating, creating, and inspecting files and directories.
## pwd
`pwd` displays the current working directory.
### Syntax
```bash
pwd
```
## ls
`ls` lists the contents of a directory.
### Syntax
```bash
ls [options] [path]
```
### Examples
```bash
ls
ls -la
```
### Important Option
```
ls -la
```
- -l     ---> Long listing format.
- -a     ---> Includes hidden files.
## cd
`cd` changes the current working directory.
### Examples
```bash
cd /
cd /home
cd ..
cd ~
```
Important Usage
- cd /    --> Move to the root directory.
- cd ..   --> Move to the parent directory.
- cd ~    --> Move to the current user's home directory.
- cd      --> Usually moves to the current user's home directory.
## tree
`tree` displays directories and files in a tree-like structure.
### Examples
```bash
tree
tree -d
```
### Important Option
```bash
tree -d
```
-d displays directories only.
- Note: tree may not be installed by default on every Linux distribution.
## touch
`touch` can create an empty file if the file does not already exist.
### Syntax
```bash
touch [options] [files]
```
### Example
```bash
touch file1
```
### Important Note
If the file alreary exists,touch normally updates its timestamps instead of creating a new file.
## mkdir
`mkdir` creates directorires.
### Syntax
```bash
mkdir [options] directory
```
### Example
```bash
mkdir folder1
mkdir folder2
```
### Creating Nested Directories
```bash
mkdir -p 1/2/3/4
```
The `-p` option allows parent directories to be created as needed.
## Spaces in File Names
The shell treats spaces as argument separators.
### For example:
```bash
touch file 1
```
is interpreted as two arguments:
```Plain text
file
1
```
To create a file whose name contains a space, quote the name:
```bash
touch "file 1"
```
This creates one file:
```Plain text
file 1
```
## Practical Examples
create a directory:
```bash
mkdir folder1
```
Move into it:
```bash
cd folder1
```
Create a file:
```bash
touch file1
```
Check the contents:
```bash
ls
```
Return to the parent directory:
```bash
cd ..
```
Check the current location:
```bash
pwd
```
## Important Concepts
- Linux paths are hierarchical.
- `/` represents the root directory.
- `..` represents the parent directory.
- `~` represents the current user's home directory.
- Spaces in filename require careful shell quoting.
- `mkdir -p` can create a complete directory hierarchy. 
