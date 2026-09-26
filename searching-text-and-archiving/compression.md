# Compression
Linux provides several tools for compressing and decompressing files.
### This document covers:
- `zip`
- `unzip`
- `gzip`
- `gunzip`
- `bzip2`
- `bunzip2`
---
## 1. zip

`zip` compresses files into a `.zip` archive.

### Compress a file
```bash
zip dictionary.zip dictionary
```
### Compress multiple files
```bash
zip textfiles.zip file1 file2
```
### Compress a directory
 Use `-r` for recursive compression:
```bash
zip -r folder1.zip folder1/
```
### Password-protected ZIP
```bash
zip -e secret.zip file1
```
 The `-e` option enables encryption and prompts for a password.
## 2. unzip
### Extract a ZIP archive:
```bash
unzip textfiles.zip
```
## 3. gzip
`gzip` compresses a file and normally replaces the original file with a compressed `.gz` file.
```bash
gzip file1
```
### The result is typically:
```Plain text
file1.gz
```
## 4. gunzip
### Decompress a `.gz` file:
```bash
gunzip file1.gz
```
### This restores:
```
file1
```
## 5. bzip2
`bzip2` uses the bzip2 compression format.
```bash
bzip2 links
```
### The result is:
```
links.bz2
```
## 6. bunzip2
### Decompress a `.bz2` file:
```bash
bunzip2 links.bz2
```
## 7. Comparison
|Tool|Operation|Typical extension|
|:---|:---:|:---:|
|zip|Compress/archive|.zip|
|unzip|Extract ZIP|.zip|
|gzip|Compress|.gz|
|gunzip|Decompress|.gz|
|bzip2|Compress|.bz2|
|bunzip2|Decompress|.bz2|
## 8. Important Note
`gzip` and `bzip2` are primarily compression tools. They are not general-purpose directory archivers by themselves.

For creating an archive containing multiple files/directories and then compressing it, Linux commonly uses `tar` together with a compression tool.

### For example:
```bash
tar -czf backup.tar.gz folder1/
```
The `tar` command creates the archive, while `gzip` provides compression.

This is covered separately in tar.md.
