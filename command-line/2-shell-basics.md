# Shell Basics: Environment, Variables and Command Execution

## 1. Identify the Current User and Shell
Display the current username:
```bash
whoami
id -un
```
Show the current login shell configured for the user:
```bash
echo "$SHELL"
```
Inspect the shell process currently running:
```bash
ps -p $$ -o comm=
```
Find the current user's account entry:
```bash
getent passwd "$USER"
```

- `/etc/passwd` contains account records for the system, not just the current user.

- `/etc/profile` contains system-wide login-shell initialization settings, commonly for Bourne-compatible shells.

- `~/.profile` and `~/.bashrc` are user configuration files; their roles and loading behavior differ.

- `echo "$SHELL"` usually shows the configured login shell, not necessarily the shell process currently running.

## 2. Navigation and Hidden Files
Print the current working directory:
```bash
pwd
```
Change to the home directory:
```bash
cd ~
```
Move to the parent directory:
```bash
cd ..
```
List files, including hidden entries:
```bash
ls -a
```
Display a configuration file:
```bash
cat ~/.profile
cat ~/.bashrc
```

> Hidden filenames normally begin with `.` .
>
>  In an interactive shell, press `Tab` to attempt filename completion; for example, type `cat ~/.pro` and press `Tab`.

## 3. Printing Output and Combining Commands
Print text:
```bash
echo "Hello, Linux"
```
A more **flexible and predictable** output command:
```bash
printf '%s\n' "Hello, Linux"
```
Execute both commands sequentially:
```bash
echo "Hello"; ip
```
Execute the second command only if the first succeeds:
```bash
echo "Hello" && ip
```
Execute the second command only if the first fails:
```bash
echo "Hello" || ip
```

- `;` runs the next command regardless of the previous command's success.

- `&&` runs the next command only if the previous command succeeds.

- `||` runs the next command only if the previous command fails.

> For scripts and reliable formatted output, prefer `printf` when its predictable behavior is useful.

## 4. Creating and Executing a Shell Script
Create a small Bash script:
```bash
cat > jimbo <<'EOF'
#!/usr/bin/env bash
printf '%s\n' "Hello from jimbo"
EOF

# Make the script executable
chmod +x jimbo

# Execute it from the current directory
./jimbo
```
### Important distinctions:
- `cat > jimbo` creates or overwrites jimbo and reads input until `Ctrl+D` signals end-of-file.

- `Ctrl+C` interrupts the current foreground operation; it is not the normal way to finish entering a file.

- `chmod +x jimbo` adds executable permission according to the existing permission settings.

- `./jimbo` explicitly runs the file from the current directory. The `./` matters because the current directory is not normally searched through `PATH`.

- A file needs valid script content and an appropriate interpreter declaration `(shebang)` to work as intended.

> Caution: `>` overwrites an existing file. Use it carefully.

## 5. Shell Variables and Environment Variables
Create a shell variable:
```bash
demo_var='example-value'
```
Read its value:
```bash
echo "$demo_var"
```
Export it to child processes:
```bash
export demo_var
```
View environment variables:
```bash
env
```
Remove the variable:
```bash
unset demo_var
```
- A shell variable is available in the **current shell**.

- `export` marks a variable to be included in the environment of subsequently executed child processes.

- `unset` removes the variable from the current shell and its exported environment.

- Variable names are case-sensitive. Use `NAME=value` without spaces around =.

- Quote variable expansions such as `$demo_var` to avoid unwanted word splitting and pathname expansion.

> Avoid using real passwords or other secrets in shell examples: environment variables and command histories are **not secure** secret-storage mechanisms.

## 6. The PATH Environment Variable
### `PATH` is a colon-separated (:) list of directories Bash searches when locating executable commands that are entered without a slash (/).
Display PATH:
```bash
printf '%s\n' "$PATH"
```
Display its directories on separate lines:
```bash
printf '%s\n' "$PATH" | tr ':' '\n'
```
Temporarily add a personal bin directory:
```bash
mkdir -p "$HOME/bin"
export PATH="$HOME/bin:$PATH"
```
Locate a command:
```bash
command -v ls
type ls
```

- Changes made in the current `shell` normally disappear when that shell ends, unless saved in a configuration file.

- Prefer a dedicated directory such as `~/bin` for personal scripts.

- Avoid adding . (the current directory) to `PATH`, because this can cause unintended commands to run.

- Use `./jimbo` when you intentionally want to execute a script in the current directory.

## 7. Command History
Display command history:
```bash
history
```
Search previous commands interactively:
```bash
Ctrl+R
```

> Bash `history` can help retrieve and reuse earlier commands. History retention and persistence depend on shell configuration.
> Avoid placing secrets directly in commands because they may be retained in history.

## 8. System Information
Display the kernel name:
```bash
uname
```
Display kernel and system information:
```bash
uname -a
```

> `uname -a` prints several system and kernel identifiers. It does not provide a complete inventory of hardware or installed software.

## 9. Manual Pages and Help
Open the manual page for a command:
```bash
man ping
```
Search manual page names and descriptions:
```bash
man -k ping
```
Equivalent search utility:
```bash
apropos ping
```
Search Bash built-in help:
```bash
help export
help cd
```
Display help for an external command when supported:
```bash
ip --help
```

- `man -k` and `apropos` search manual-page names and short descriptions.

- `man` displays the installed manual page, if available.

- `help` is especially useful for Bash built-ins such as cd, export, and exec.

- Help options vary by command; some programs use `--help`, while others provide different mechanisms.

## 10. The exec Built-in
```bash
exec ls -a
```

> When `exec` is used with an external command, Bash replaces the current shell process with that command rather than launching it as a normal child process.
> When ls finishes, the replaced shell does not resume.
> Therefore, do not use this example in an interactive terminal unless you intend to replace that shell session.
>
> Without a command, `exec` can also be used to change file descriptors through redirection.

### Key Takeaways
- Distinguish the current user, configured **login shell**, and **running shell** process.

- Understand the difference between **shell variables** and **exported environment variables**.

- Use `./script` for intentional execution from the current directory instead of adding . to PATH.

- Use `Ctrl+D` to finish terminal input to `cat`; `Ctrl+C` interrupts.

- Use `man`, `apropos`, `type`, and `command -v` to investigate commands.

