# Linux Navigation
Commands used to navigate through the Linux filesystem.
## pwd
`pwd` display the current working directory.
### syntax
```bash
pwd
```
## cd
`cd` changes the current working directory.
### syntax
```bash
cd /
cd /bin
cd /usr/bin
cd ..
cd ~
cd
```
## Important Usage
- cd /     --> Go to the root directory.
- cd ..    --> Move to the parent directory.
- cd ~     --> Go to the current user's home directory.
## ls
`ls` lists directory contents.
### syntax
```bash
ls
ls -la
```
## PATH
the `PATH` environment variable contains directories that the shell searches when looking for executable commands.
### syntax
```bash
echo $PATH
```
Example command locations include:
```plain text
/bin
/usr/bin
/usr/local/bin
/sbin
/usr/sbin
/usr/local/sbin
```
## Command Location
```bash
type ping
whereis ping
```
`type` can show how the shell interprets a command, while `whereis` can locate related files such as binaries and documentation.
