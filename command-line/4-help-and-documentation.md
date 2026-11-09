# Linux Help and Documentation
Linux provides several mechanism for finding information about commands.
## man
Displays the manual page for a command.
```bash
man cat
man ping
```
## --help
Many commands provide a short usage guide.
```bash
ping --help
```
The output can be viewed page by page:
```bash
ping --help | less
```
## whatis
Displays a short description of a command.
```bash
whatis ping
```
## apropos
Searches manual page descriptions for a keyword.
```bash
apropos ping
```
## man -k
Searches manual page names and descriptions for a keyword.
```bash
man -k ping
```
## info
Displays GNU Info documentation when available.
```bash
info ping
```

### Practical Approach
When I need to learn an unfamiliar command, I can use :
1. command `--help`
2. `man` command
3. `whatis` command
4. `apropos` keyword
5. `info` command when available

### Important Note
Manual pages are one of the most important sources of technical information on Linux systems.
