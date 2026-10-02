# Bash Shell Scripting Basics
## Shebang
A Bash script commonly starts with:
```bash
#!/bin/bash
```
## Creating and Running a Script
```bash
nano script.sh
```
Make it executable:
```bash
chmod +x script.sh
```
Run it:
```bash
./script.sh
```
A script can also be executed directly with Bash:
```bash
bash script.sh
```
## Variables
```bash
NAME="Hesam"
echo "$NAME"
```
## Command-Line Arguments
Important special variables:
```
$0   Script name
$1   First argument
$2   Second argument
$#   Number of arguments
$@   All arguments
$$   PID of current shell
```
Example:
```bash
echo "Script: $0"
echo "First argument: $1"
echo "Arguments: $#"
echo "All arguments: $@"
```
## User Input
```bash
read YOURNAME
echo "Hello $YOURNAME"
```
With a prompt:
```bash
read -p "Please enter your name: " name
```
## Environment Variables
Examples:
```bash
echo "$HOSTNAME"
echo "$USER"
echo "$HOME"
```
## Conditional Statements
```bash
if [[ "$name" == "$correct" ]];
then
    echo "Correct"
else
    echo "Incorrect"
fi
```
## File Tests
```bash
if [ -f "$1" ];
then
    echo "File exists"
else
    echo "Not a file"
fi
```
## Command Substitution
```bash
COUNT=$(wc -l < "$1")
```
The output of a command can be stored in a variable.
## For Loop
```bash
for i in {1..10}
do
    echo "$i"
done
```
## While Loop
```bash
while [ "$INPUT" != "hesam" ]
do
    read INPUT
done
```
## Until Loop
```bash
until false
do
    echo "Running"
    sleep 2
done
```
This creates an infinite loop because the condition never becomes true.
## Functions
```bash
allfiles() {
    ls -al ~/Desktop/
}

allfiles
```
## Logical Operators
```bash
command && echo "Success"
command || echo "Failed"
```
`&&` executes the second command when the first succeeds.
`||` executes the second command when the first fails.
