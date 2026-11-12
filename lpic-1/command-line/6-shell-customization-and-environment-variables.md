# Shell Customization and Environment Variables

## 1. Shell Types
A `Bash` shell can be `Login` or `Non-Login`, and `Interactive` or `Non-Interactive`.

Type|Description
|---|---|
Login Shell|Started as a login session
Non-Login Shell|Started inside an existing session
Interactive Shell|Accepts commands directly from the user
Non-Interactive Shell|Runs commands from scripts or other sources

Important: `Login` and `Interactive` are independent properties.

## 2. Bash Configuration Files

### System-Wide Configuration
File / Directory|Purpose
|---|---|
`/etc/profile`|Login shell configuration
`/etc/profile.d/`|Additional system-wide profile scripts
`/etc/bash.bashrc`|Interactive Bash configuration (Debian/Ubuntu)
`/etc/bashrc`|Common system-wide Bash configuration on some distributions

### User Configuration
File|Purpose
|---|---|
`~/.bash_profile`|User login configuration
`~/.bash_login`|Alternative login configuration
`~/.profile`|User login configuration
`~/.bashrc`|Interactive shell configuration
`~/.bash_logout`|Commands executed when a login shell exits

### Important Notes :

- Bash checks `~/.bash_profile`, `~/.bash_login`, and `~/.profile` in this order.

- Only the first existing and readable file is used.

- `/etc/profile` commonly loads scripts from `/etc/profile.d/`.

- `/etc/skel/` contains default files copied into newly created users' home directories.

## 3. Aliases
Aliases are shortcuts for commands.
### Syntax
```bash
alias NAME='COMMAND'
```
Example :
```bash
alias ll='ls -lah'
alias test='ping'
```
List aliases :
```bash
alias
```
Remove an alias :
```bash
unalias ll
```
> Aliases can be added to `~/.bashrc` for future interactive Bash sessions.

## 4. Shell Functions
Functions allow multiple commands to be grouped under one name.
### Syntax
```bash
function_name() {
    COMMANDS
}
```
Example :
```bash
hesam() {
    echo "hesam is an LPIC student."
    whoami
}
```
Run the function :
```bash
hesam
```
> Functions can be defined in `~/.bashrc` or system-wide Bash configuration files.

## 5. Shell Variables and Environment Variables
### Shell Variables
A shell variable is available in the current shell.
```bash
hesam="I am hesam"
echo "$hesam"
```
### Environment Variables
Environment variables are exported to child processes.
```bash
export hesam="I am hesam"

echo "$hesam"
env
printenv hesam
```

### Important Commands

Command|Purpose
|---|---|
`set`|Display shell variables and functions
env|Display environment variables
printenv|Display environment variables
export NAME|Export an existing variable
unset NAME|Remove a variable
echo "$NAME"|Display a variable's value

## 6. PATH Environment Variable
`PATH` contains directories where the shell searches for executable commands.

Display PATH :
```bash
echo "$PATH"
```
Add a directory temporarily :
```bash
export PATH="$PATH:$HOME/bin"
```
This change affects the current shell and its child processes.

> To make it persistent, add the export command to an appropriate startup file, such as `~/.profile`.

## 7. Running Shell Scripts
Example script :
```bash
#!/bin/bash

uptime
ip
ls
```
Run the script :
```bash
# Make executable
chmod +x script.sh

# Execute from current directory
./script.sh

# Execute using Bash
bash script.sh
```

### Important:
- `#!/bin/bash` is called the Shebang.

- `./script.sh` executes a file using its relative path.

- A command can be executed by name without `./` if its directory is included in `PATH`.

- The current directory is not normally included in `PATH`.

## 8. Reload Configuration Files
After editing `~/.bashrc`, apply changes to the current shell :
```bash
source ~/.bashrc
```
Equivalent command :
```bash
. ~/.bashrc
```

## 9. Quick Reference

Requirement|Location / Command
|---|---|
System-wide login settings|`/etc/profile`
User login settings|`~/.profile`
User aliases and functions|`~/.bashrc`
Logout commands|`~/.bash_logout`
Default files for new users|`/etc/skel/`
Display shell variables|`set`
Display environment variables|`env`
Display PATH|`echo "$PATH"`
Export a variable|`export NAME`
Reload Bash configuration|`source ~/.bashrc`

## 10. Important Notes

- Login shells and interactive shells are different concepts.

- Configuration files are loaded depending on how Bash is started.

- Changes to shell variables do not automatically affect parent processes.

- Exported variables are inherited by child processes.

- Aliases and functions are commonly stored in `~/.bashrc`.

- PATH allows commands to run without specifying their full location.
