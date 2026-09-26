# tar

The `tar` command is used to create and extract archives.

`tar` itself is primarily an archiving tool. Compression can be added using gzip or bzip2.

---

## 1. Create an Archive

### Basic syntax:

```bash
tar -cf archive.tar file1
```
- `-c` → create archive
- `-f` → specify archive filename
### Archive multiple files
```bash
tar -cf textfile.tar file1 file2
```
### Archive a directory
```bash
tar -cf folder.tar folder1/
```
## 2. Verbose Mode
### Use `-v` to display the files being processed:
```bash
tar -cvf textfile.tar file1
```
- `-v` → verbose
## 3. Extract an Archive
```bash
tar -xf textfile.tar
```
- `-x` → extract
- `-f` → specify archive filename
### Verbose extraction:
```bash
tar -xvf textfile.tar
```
## 4. tar + gzip
### Create a gzip-compressed archive:
```bash
tar -zcvf textfile.tar.gz file1
```
### Extract it:
```bash
tar -zxvf textfile.tar.gz
```
- `-z` → use gzip compression
## 5. tar + bzip2
### Create a bzip2-compressed archive:
```bash
tar -jcvf textfile.tar.bz2 file1
```
### Extract it:
```bash
tar -jxvf textfile.tar.bz2
```
- `-j` → use bzip2 compression
## 6. Common Options
|Option|Meaning|
|:---|:---:|
|-c|	Create archive|
|-x|	Extract archive|
|-f|	Archive filename|
|-v|	Verbose output|
|-z|	gzip|
|-j|	bzip2|
## 7. Archive vs Compression
These are different concepts.
### Archive
Combines multiple files/directories into one archive:
```bash
tar -cf backup.tar folder1/
```
### Compression
Reduces the size of data:
```bash
gzip file1
```
### Archive + Compression
```bash
tar -czf backup.tar.gz folder1/
```
### Here:
```
tar
 ↓
creates archive
 ↓
gzip
 ↓
compresses archive
```
## 8. Practical Examples
### Create a compressed archive:
```bash
tar -czf backup.tar.gz folder1/
```
### Extract it:
```bash
tar -xzf backup.tar.gz
```
### Create a bzip2 archive:
```bash
tar -cjf backup.tar.bz2 folder1/
```
### Extract it:
```bash
tar -xjf backup.tar.bz2
```

## Important Notes
- `tar` is primarily an archiving tool.
- `gzip` and `bzip2` provide compression.
`.tar.gz` means a tar archive compressed with gzip.
`.tar.bz2` means a tar archive compressed with bzip2.
