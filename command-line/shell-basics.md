# Shell Basics
Basic commands and concapts for interacting with the Linux shell.
## touch
Creates an empty file if it does not already exist.
```bash
touch file
```
## cat
Display the contents of a file.
```bash
cat file
```
## clear
Clears the terminal screen.
```bash
clear
```
## echo
Prints text or the value of a variable.
```bash
echo Hello
echo "My name is; Hesam"
echo "My salary is \$2000 per month"
```
## Shell Variables
A variable can be assigned using:
```bash
a='Hesam Khavari'
```
The value can be displayed using:
```bash
echo "$a"
```
- Note : There should be no spaces around = during a  normal Bash variable assignment.
## Environment Variables
```bash
printenv
echo $PATH
```
## History
The shell keeps a `history` of previously executed commands.
```bash
history
history 5
```
To clear the current shell history :
```bash
history -c
```
## Special Keys
- `Ctrl+C`
Interrupts the currently running foreground command.
- `Ctrl+Z`
Suspends the current foreground job.
- `Ctrl+Alt+T`
Common desktop shortcut for opening a terminal emulator.
- `Ctrl+Alt+F1...F6`
Can switch between virtual terminals on systems/configurations that provide these TTYs.
- `TTY`
The tty command displays the name of the terminal connected to standard input.
```bash
tty
```
