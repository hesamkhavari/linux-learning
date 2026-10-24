# Standard Streams, Pipes and Redirection

## 1. Standard Streams
### Linux commands normally use three standard streams:
Stream|File descriptor|Purpose
|---|:---:|---|
Standard Input (stdin)|0|Input to a command
Standard Output (stdout)|1|Normal command output
Standard Error (stderr)|2|Error messages

## 2. Standard Input
### The `<` operator redirects a file into a command's **standard input**.
```bash
wc file1.txt
wc < file1.txt
```

Both count lines, words, and bytes. However, `wc file1.txt` normally prints the filename, while `wc < file1.txt` reads from standard input and omits the filename.

```bash
wc *
```

Counts lines, words, and bytes for each matched file and prints a total where applicable.

## 3. Output Redirection
### Overwrite a file :
```bash
ls > list.txt
ls 1> list.txt
```

Both redirect standard output to `list.txt`. If the file exists, it is truncated first.

### Append to a file :
```bash
ls >> list.txt
```

Appends standard output instead of overwriting existing content.

### Redirect standard error :
```bash
ls *.mp4 2> mp4-errors.txt
ls *.mp4 2>> mp4-errors.txt
```
- `2>` writes standard error to a file, replacing its contents.
- `2>>` appends standard error to a file.

If the pattern matches no files, the shell and command behavior depends on the shell's globbing rules.
In Bash's default configuration, `ls` typically reports the unmatched pattern as an error.

## 4. Redirecting Output and Errors Together
```bash
ls *.mp4 > output.txt 2>&1
```

This sends both standard output and standard error to `output.txt`.

The order matters: `2>&1` makes standard error use the destination currently assigned to standard output.

### To append both streams :
```bash
ls *.mp4 >> output.txt 2>&1
```

Important: Use `2>&1`, not 2> &1.

## 5. Pipes
The pipe operator `|` connects the **standard output** of one command to the **standard input** of another.
```bash
ls *.txt | grep it
```

This passes the output of `ls` to `grep`, which selects lines containing it.

```bash
ls *.txt | echo
```
This does not print the filenames as expected because `echo does not read its standard input`.

```bash
ls *.txt | xargs echo
```
`xargs` reads input and builds command arguments. 
However, whitespace and special characters in filenames can cause problems with this simple form.

For robust filename handling, prefer `null-delimited` input, for example:
```bash
find . -maxdepth 1 -type f -name "*.txt" -print0 | xargs -0 printf '%s\n'
```
For ordinary text processing, use commands that actually read standard input, such as `grep`, `sort`, `wc`, and `cut`.

## 6. The tee Command
`tee` reads standard input, writes it to standard output, and also saves it to a file.
```bash
ls *.txt | tee file-list.txt
```
Displays the matching names and writes them to `file-list.txt`.
```bash
ls *.txt | tee -a file-list.txt
```
The `-a` option appends to the file instead of overwriting it.
## 7. Common Redirection Operators

Operator|Meaning
|---|---|
|< file|Read standard input from a file
|> file|Redirect standard output; overwrite
|>> file|Redirect standard output; append
|2> file|Redirect standard error; overwrite
|2>> file|Redirect standard error; append
|2>&1|Send standard error to the current standard-output destination
|\||Pipe standard output to another command
|tee file|Write input to both standard output and a file
|tee -a file|Append input to a file and also display it
