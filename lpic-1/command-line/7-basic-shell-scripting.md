# Basic Shell Scripting

## 1. Introduction to Shell Scripts
A shell script is a file that contains commands. The shell runs these commands one by one.

### Create a Script
```bash
mkdir -p ~/scripts
cd ~/scripts
touch script.sh
nano script.sh
```
### Basic Script Structure
```bash
#!/bin/bash

# This is a comment
echo "Hello World"
```

- `#!/bin/bash` : Defines Bash as the interpreter.

- `#` : Starts a comment.

- `echo` : Prints text on the screen.

### Run a Script
```bash
chmod +x script.sh
./script.sh
```
Another way :
```bash
bash script.sh
```

## 2. Functions vs Shell Scripts

Function|Shell Script
|---|---|
A block of commands with a name|A file containing commands
Usually runs in the current shell|Usually runs in a separate process
Can be loaded from .bashrc|Can be executed as a program

> Note: `source script.sh` runs a script in the current shell.

## 3. File Permissions and Exit Status
Make a script executable :
```bash
chmod u+x script.sh
chmod +x script.sh
```
Check permissions :
```bash
ls -l script.sh
```
### Test Command
The `test` command checks a condition.
```bash
test -x script.sh
echo $?

test -e file.txt
echo $?
```

- `0` : Condition is true / command succeeded.

- `Non-zero` : Condition is false / command failed.

### Common File Tests

Operator|Meaning
|---|---|
`-e`|File or directory exists
`-f`|Regular file exists
`-d`|Directory exists
`-x`|Executable
`-r`|Readable
`-w`|Writable

## 4. Conditional Statements (if)
The `if` statement runs commands based on a condition.
### Syntax
```bash
if [ CONDITION ]; then
    COMMANDS
else
    COMMANDS
fi
```
### Example
```bash
#!/bin/bash

echo "Are you an engineer?"
read -r ISNAME

if [ "$ISNAME" = "Yes" ]; then
    echo "Very nice! I am proud of you."
else
    echo "Keep learning!"
fi
```

### Important Comparison Operators

|Operator|Meaning|
|---|---
=|Strings are equal
!=|Strings are not equal
-eq|Numbers are equal
-ne|Numbers are not equal
-gt|Greater than
-lt|Less than
-ge|Greater than or equal
-le|Less than or equal

> Important: Spaces around [ and ] are required. Put variables inside quotes, such as `"$ISNAME"`.

## 5. For Loops
A `for` loop repeats commands for each item in a list.
### Syntax
```bash
for VARIABLE in LIST
do
    COMMANDS
done
```
### Example 1: List of Numbers
```bash
#!/bin/bash

for x in 1 2 3 4 5
do
    echo "$x"
done
```
### Example 2: Brace Expansion
```bash
for x in {5..10}
do
    echo "$x"
done
```
### Example 3: Arithmetic For Loop
```bash
for ((x=1; x<=5; x++))
do
    echo "$x"
done
```
### Example 4: Loop Through Files
```bash
for file in "$HOME"/*
do
    [ -e "$file" ] || continue
    echo "$file"
done
```

> Note: Avoid using `files=$(ls ~)` to create a list of filenames. It can fail with filenames containing spaces or special characters.

## 6. While Loops
A `while` loop repeats commands as long as its condition is true.
### Syntax
```bash
while [ CONDITION ]
do
    COMMANDS
done
```
### Example 1: Count from 1 to 4
```bash
#!/bin/bash

x=1

while [ "$x" -ne 5 ]
do
    echo "$x"
    x=$((x + 1))
done
```
### Example 2: Read User Input
```bash
#!/bin/bash

echo "Type something or type stop."

while true
do
    read -r x

    if [ "$x" = "stop" ]; then
        break
    fi

    echo "$x"
done
```

- `break` : Stops the loop.

- `continue` : Skips the current iteration.

## 7. Variables and Arithmetic
Create a variable :
```bash
name="Hesam"
x=5
```
Use a variable :
```bash
echo "$name"
echo "$x"
```
Arithmetic expansion :
```bash
echo $((5 + 2))
echo $((5 - 2))
echo $((5 * 2))
echo $((10 / 2))

x=$((x + 1))
```

> Note: Do not use spaces around `=` when assigning a variable.

## 8. Useful Script Commands

Command|Purpose
|---|---|
`read -r VAR`|Read user input
`echo`|Print text
`printf`|Print formatted output
`test`|Check a condition
`chmod +x FILE`|Add executable permission
`exit 0`|Exit successfully
`exit 1`|Exit with an error status
`source FILE`|Run a file in the current shell
`break`|Exit a loop
`continue`|Skip the current loop iteration

### Script Debugging
Check syntax without running the script :
```bash
bash -n script.sh
```
Run with debugging information :
```bash
bash -x script.sh
```

## 9. Important Notes

- A script usually runs in a separate process.

- A function usually runs in the current shell.

- Use `$(COMMAND)` instead of old-style `backticks`.

- Use `"$VARIABLE"` to protect spaces and special characters.

- Use `read -r` when reading user input.

- Use `#!/bin/bash` to select Bash as the interpreter.

- SUID `chmod u+s` is not a safe or reliable way to grant privileges to Bash scripts.
