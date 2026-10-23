# File Management and Archiving

## 1. Copy, Move and Remove

Command|Purpose
|---|---|
cp source destination|Copy a file
cp -R dir1 dir2|Copy a directory recursively
cp -i file destination|Prompt before overwriting
mv source destination|Move or rename a file
mv -i source destination|Prompt before overwriting
rm -i file|Prompt before deleting
rm -r directory|Remove a directory recursively
mkdir directory|Create a directory
touch file|Create an empty file or update timestamps
file filename|Identify file type
ls -lh|List files with human-readable sizes

> Safety: Avoid `rm -rf` unless the target path has been carefully verified. It can delete files recursively without prompting.

## 2. Shell Globbing
### Globbing patterns are expanded by the shell before a command runs.

Pattern|Meaning
|---|---|
`*.txt`|Names ending in .txt
`*.???`|Names with any extension of exactly three characters
`???`|Names containing exactly three characters
`[ac]*`|Names beginning with a or c
`[a-c]*`|Names beginning with a, b, or c
`[!a-c]*`|Names not beginning with a, b, or c

Example :
```bash
ls *.txt
ls [a-c]*
```

> **Globbing** is not the same as **find** : shell patterns usually match names in the current directory, while find can search recursively.

## 3. Find Files and Directories
```bash
find . -name "*.jpg"
find . -name "[a-c]*.*"
find . -type f
find . -type d
find . -type f -name "*.jpg" -size +1M
find . -type f -size -3M
```

- `.` starts searching from the current directory.

- `-name` matches a filename pattern.

- `-type f` selects regular files.

- `-type d` selects directories.

- `-size +1M` matches files larger than the specified size threshold.

- `-size -3M` matches files below the specified threshold.

Time-based searches :
```bash
find . -type f -mtime +1
find . -type f -atime +2
find . -type f -ctime +5
```

- `-mtime` : time since file contents were last modified.

- `-atime`: time since the file was last accessed.

- `-ctime`: time since file status/metadata last changed; it does not mean creation time.

> For these day-based tests, GNU find counts complete 24-hour periods, so `+n` is not simply “within the last n days.” See the GNU find reference.

## 4. CPIO Archives
### `cpio` creates or extracts archives using a list of filenames supplied through standard input.
Create an archive containing ZIP files found under the current directory :
```bash
find . -depth -type f -name "*.zip" -print0 | cpio --null -o > zipfiles.cpio
```
List the archive contents :
```bash
cpio -it < zipfiles.cpio
```
Extract into the current directory :
```bash
cpio -id < zipfiles.cpio
```

### Options:
- `-o`: copy-out mode; create an archive.

- `-i`: copy-in mode; extract an archive.

- `-t`: list archive contents.

- `-d`: create directories as needed.

- `--null`: use null-delimited filenames; useful for filenames containing spaces or newlines.

> `cpio` archives files but does not inherently compress them. See the GNU cpio manual.

## 5. Disk Space and dd
```bash
df -h
```
> Shows filesystem space usage in human-readable units.

### `dd` copies data from an input (if=) to an output (of=), including raw device data.
Example of creating an image file from a device :
```bash
sudo dd if=/dev/sdX of=usb-storage.img bs=4M status=progress
```

> Critical: Replace /dev/sdX only after identifying the correct source device.
> Reversing `if=` and `of=` can overwrite a disk and destroy data.
> Never experiment with real disks or USB devices containing important data.

## 6. Compression
```bash
gzip usb-storage.img
gunzip usb-storage.img.gz

bzip2 usb-storage.img
bunzip2 usb-storage.img.bz2
```

### By default, `gzip` and `bzip2` replace the original file with its compressed version. Decompression restores the uncompressed file.

## 7. Tar Archives
Create an uncompressed archive :
```bash
tar -cvf files.tar ./files
```
List its contents :
```bash
tar -tvf files.tar
```
Create compressed archives :
```bash
tar -czvf files.tar.gz ./files
tar -cjvf files.tar.bz2 ./files
```
Extract archives :
```bash
tar -xvf files.tar
tar -xvf files.tar.gz
tar -xvf files.tar.bz2
```

### Common options :

Option|Meaning
|---|---|
`-c`|Create an archive
`-x`|Extract an archive
`-t`|List archive contents
`-v`|Show processed filenames
`-f`|Specify archive filename
`-z`|Use gzip compression
`-j`|Use bzip2 compression

> Modern GNU tar can automatically detect compression when reading an archive, so `tar -xf archive.tar.gz` is generally sufficient. See the GNU tar manual.

> Good practice: List an archive's contents before extracting it, and extract untrusted archives into a dedicated directory.
