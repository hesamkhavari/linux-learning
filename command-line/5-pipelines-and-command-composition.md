# Pipelines and Command Composition

Linux commands can be combined to build more useful command-line operations.

This document covers:

- Pipes `|`
- Command substitution `$(...)`
- `xargs`
- Input/output redirection

---

## 1. Pipe `|`

A pipe sends the standard output of one command to the standard input of another command.

### Basic syntax:

```bash
command1 | command2
```
Example:
```bash
cut -d":" -f1 /etc/passwd | sort
```
This:
1. Extracts usernames from /etc/passwd
2. Sends the output to sort
3. Displays the sorted result

Another example:
```bash
seq 7 | xargs echo
```
## 2. Command Substitution

Command substitution allows the output of a command to be used as part of another command.

Syntax:
```bash
$(command)
```
Example:
```bash
echo "Today is $(date)"
```
The command inside `$()` runs first, and its output is inserted into the surrounding command.
## 3. xargs
`xargs` reads items from standard input and uses them as arguments for another command.
### Example:
```bash
seq 7 | xargs echo
```
The output of `seq` is passed as arguments to `echo`.
### Another example:
```bash
cut -d":" -f1 /etc/passwd | xargs sort
```
The extracted usernames are passed as arguments to `sort`.
## 4. Input Redirection
The `<` operator redirects a file to the standard input of a command.
### Example:
```bash
sort < file1
```
Instead of reading from the terminal, `sort` receives its input from `file1`.
## 5. Output Redirection
The `>` operator redirects standard output to a file.
```bash
echo "Hello Linux" > file1
```
If `file1` already exists, its contents are overwritten.
## 6. Append Output
The `>>` operator appends output to a file instead of replacing its contents.
```bash
echo "Another line" >> file1
```
This adds the output to the end of `file1`.
## 7. Combining Commands
Linux commands become much more powerful when combined.
### Example:
```bash
cut -d":" -f1 /etc/passwd | sort
```
### Another example:
```bash
find . -name "*.txt" | xargs
```
The output of `find` becomes input for the next command.

## 8. Important Notes
- `|` connects the output of one command to the input of another.
- `$(...)` performs command substitution.
- `xargs` converts input into command arguments.
- `>` overwrites the destination file.
- `>>` appends to the destination file.
- `<` redirects a file to standard input.

### These techniques form the foundation for combining Linux command-line tools.
