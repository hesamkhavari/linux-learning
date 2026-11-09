# Text Processing
Linux provides several command-line tools for viewing and processing text files.
This document covers:
- `cat`
- `head`
- `tail`
- `cut`
- `sort`
- `wc`
These commands are especially useful when working with configuration files, logs, command output, and structured text.
---
## 1. cat
The `cat` command is commonly used to display the contents of a file.
### Basic syntax
```bash
cat FILE
```
Example:
```bash
cat file1
```
This displays the contents of file1.
## 2. head
The `head` command displays the beginning of a file.
```bash
head file1
```
By default, head displays the first 10 lines.
### Specify the number of lines
```bash
head -n 20 file1
```
This displays the first 20 lines.
`-n` specifies the number of lines to display.
## 3. tail
The `tail` command displays the end of a file.
```bash
tail file1
```
By default, tail displays the last 10 lines.
### Specify the number of lines
```bash
tail -n 20 file1
```
This displays the last 20 lines.
## 4. head vs tail
The main difference is the part of the file they display.
|Command|Purpose|
|:---:|:---:|
|head file1|Beginning of the file|
|tail file1|End of the file|

### For example:
```bash
head -n 20 file1
```
displays the first 20 lines.
### While:
```bash
tail -n 20 file1
```
displays the last 20 lines.
## 5. cut
The `cut` command is used to extract specific sections of each line.
### It can work with:
- Character positions
- Fields
- Delimiters
## 6. Extract characters
Use `-c` to select character positions.
```bash
cut -c3-8 file1
```
This extracts characters from position 3 through position 8 from each line.
### Select specific characters
```bash
cut -c3,8 file1
```
This extracts characters 3 and 8 from each line.
### Extract from a specific character to the end
```bash
cut -c13- file1
```
This extracts characters starting at position 13 through the end of each line.
## 7. Extract Fields
The `-d` option specifies the delimiter.
The `-f` option specifies which field to extract.
Example:
```bash
cut -d" " -f2 file1
```
This uses a space as the delimiter and extracts the second field.
### Extract multiple fields
```bash
cut -d" " -f2-4 file1
```
This extracts fields 2 through 4.
## 8. Example with /etc/passwd
The /ets/passwd file uses : as a delimiter.
### For example:
```Plain text
username:x:1000:1000:User Name:/home/username:/bin/bash
```
The first field contains the username.
### We can extract it using:
```bash
cut -d":" -f1 /etc/passwd
```
Explanation:
- `-d":" --> use : as the delimiter
- `-f1`  --> select the first field
## 9. wc
The `wc` command counts information about text.
```bash
wc file1
```
By default, itdisplays:
- Number of line
- Number of words
- Number of bytes
Example output:
```Plain text
10 25 150 file1
```
This means:
- 10 --> lines
- 25 --> words]
- 150 --> bytes
## 10. Count Lines
Use `-l` :
```bash
wc -l file1
```
This displays the number of lines.
## 11. Count Words
Use `-W` :
```bash
wc -W file1
```
This displays the number of words.
## 12. Count Bytes
Use `-c` :
```bash
wc -c file1
```
This displays the number of bytes.
## 13. sort
The `sort` command sorts lines of text.
### Example:
```bash
sort file1
```
This sorts the lines in ascending lexicographical order.
### Reverse serting
```bash
sort -r file1
```
The `-r` option reverses the sorting order.
## 14. Combining Commands
Linux commands can be combined using pipes.
### For example:
```bash
wc file1 | cut -d" " -f1
```
The output of `wc` is passed to `cut`.
### Another example:
```bash
cut -d":" -f1 /etc/passwd | sort
```
This:
1. Extracts usernames from /etc/passwd
2. Passes them to sort
3. Displays the usernames in sorted order
## 15. Practical Examples
### View the first lines of a log file
```bash
head /var/log/example.log
```
### View the last lines of a log file
```bash
tail /var/log/example.log
```
### Count lines in a configuration file
```bash
wc -l /path/to/config
```
### Extract usernames
```bash
cut -d":" -f1 /etc/passwd
```
### Sort usernames
```bash
cut -d":" -f1 /etc/passwd | sort
```
## 16. Important Notes
- `cat` displays file contents.
- `head` displays the beginning of a file.
- `tail` displays the end of a file.
- `cut` extracts characters or fields.
- `sort` sorts lines.
- `wc` counts lines, words, and bytes.
- Pipes allow the output of one command to become the input of another command.

These commands become especially useful when combined with `grep`, `find`, pipes, and other command-line tools.
## 17. Real-World Administration
Text-processing commands are frequently used when troubleshooting Linux systems.
### For example:
```bash
grep "error" /var/log/example.log | tail
```
This searches for lines containing error and then displays the last matching lines.
### Another example:
```bash
cut -d":" -f1 /etc/passwd |sort
```
This extracts usernames from `/etc/passwd` and sorts them.

### The power of Linux command-line administration aften comes from combining small tools together rather than relying on one large command.
