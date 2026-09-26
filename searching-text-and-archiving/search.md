# Searching for Files and Text
Linux provides several commands for searching files, directories, and text.
Two important commands are:
- `find` --> search for files and directories
- `grep` --> search for text patterns
---
## 1. find
The `find` command is used to search for files and directories in a directory hierarchy.
### Basic syntax
```bash
find PATH [OPTIONS] [EXPRESSION]
```
## 2. Search from the Current Directory
```bash
find .
```
### The `.` means the current directory.
This searches recursively through the current directory and its subdirectories.
## 3. Search from the Root Directory
```bash
find /
```
### This searches the entire filesystem hierarchy starting from `/`.
Depending on the system, this may produce a large amount of output and may require elevated privileges to access some directories.
## 4. Find Files
Use `-type f` to search only for regular files.
```bash
find . -type f
```
Example:
```Plain text
./file1
./documents/file2
./documents/test.txt
```
## 5. Find Directories
Use `-type d` to search only for directories.
```bash
find . -type d
```
## 6. Search by Name
Use `-name` to search for a specific filename pattern.
```bash
find . -name "file1"
```
### The search is case-sensitive.
For example:
```bash
find . -name "file*"
```
This can match names such as:
```Plain text
file1
file2
file.txt
files
```
The `*` is a wildcard that matches zero or more characters.
## 7. Practical Examples
Find all `.txt` files:
```bash
find . -name "*.txt"
```
Find directories whose names start with test:
```bash
find . -type d -name "text*"
```
Find regular files whose names start with file:
```bash
find . -type f -name "file*"
```
---
## grep
## 8. What is grep?
The `grep` command searches for text matching a pattern inside files or input.
### Basic syntax:
```bash
grep [OPTIONS] PATTERN [FILE...]
```
## 9. Search for Text in a File
For example:
```bash
grep aa file1
```
### This searches for the text ``aa` inside file1.
Only lines containing the matching pattern are displayed.
## 10. Search Multiple Files
```bash
grep is ./*
```
This searches for the pattern `is` in files in the current directory.
## 11. Search at the Beginning of a Line
The `^` character represents the beginning of a line in a regular expression.
```bash
grep "^ran" dictionary
```
This matches lines that start with:
```Plain text
ran
```
## 12. Search at the End of a Line
The `$` character represents the end of a line.
```bash
grep "ran$" directory
```
This matches lines that end with:
```Plain text
ran
```
## 13. Using the Dot .
### In a basic regular expression, `.` matches any single character.
For example:
```bash
grep "a.b" file1
```
This can match string such as:
```Plain text
aab
acb
a1b
```
because the character between a and b can vary.
## 14. Character Classes
### Square brackets can define a set or range of characters.
Example:
```bash
grep "^c[a-d]t" dictionary
```
This can match:
```Plain text
cat
cbt
cct
cdt
```
### because `[a-d]` represents one character from a through d.
Another example:
```bash
grep "c[aeiou]t" directory
```
### This matches c, followed by one vowel, followed by t.
For example:
```Plain text
cat
cet
cit
cot
cut
```
## 15. Character Ranges
You can also specify a range:
```bash
grep "^c[a-zA-Z]" dictionary
```
This matches lines beginning with c followed by an English alphabetic character.
## 16. Exact-Length Pattern
The following pattern:
```bash
grep "^...$" dictionary
```
### matches lines containing exactly three characters.

Explanation:
- ^ --> beginning of line
- . --> any single character
- $ --> end of line

Therefore the complete pattern represents exactly three characters.
## 17. Important Regular Expression Characters
|Character|Meaning|
|:---:|:---:|
|^|Beginning of line|
|$|End of line|
|.|Any single character|
|*|Zero or more repetitions|
|[abc]|One character from a, b or c|
|[a-z]|One character from a through z|

## 18. find vs grep
These commands solve different problems:

### find
Used to search for files and directories:
```bash
find . -type f -name "*.txt"
```
Think:
 Where is the file?

### grep
Used to search for text:
```bash
grep "error" logfile
```
Think:
 Where is this text?
## 19. Combining find and grep
