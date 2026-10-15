# Shared Libraries and Dynamic Linking

## 1. Static vs Shared Libraries
- **Static Library** : Library code is linked into the executable during compilation. The executable may still depend on other shared libraries.
- **Shared Library** : Loaded at runtime by the dynamic linker. Multiple programs can use the same library.

## 2. Library Files and Paths
Shared libraries commonly use the .so extension.

Example:
```Plain text
libexample.so -> libexample.so.1 -> libexample.so.1.2
```
These names commonly represent the development link, ABI/SONAME version, and a specific library build. Naming conventions vary.

Common library directories:
```Plain text
/lib/
/usr/lib/
/lib64/
/usr/local/lib/
```
Exact paths depend on the distribution and architecture; some systems merge `/lib` into `/usr/lib`.

## 3. Dynamic Linker Configuration
- `/etc/ld.so.conf`: Main configuration file.

- `/etc/ld.so.conf.d/`: Additional configuration files.

- `/etc/ld.so.cache`: Cache of shared-library locations used by the dynamic linker.

- `ldconfig`: Updates library links and the cache.

## 4. Useful Commands
Show a program's shared-library dependencies:
```bash
ldd /bin/ping
```
Display configured library paths and cache entries:
```bash
cat /etc/ld.so.conf
ldconfig -p
```
Refresh the cache after configuring library paths:
```bash
sudo ldconfig
```
Inspect the dynamic linker and related files:
```bash
ls -l /etc/ld.so.conf.d/
ls -l /lib/
ls -l /usr/lib/
```

## 5. Environment Variables
Display environment variables:
```bash
printenv
```
Set a variable for the current shell and its child processes:
```bash
export MY_VAR="example"
```
Temporarily add custom library directories to the runtime search path:
```bash
export LD_LIBRARY_PATH="$HOME/lib:$HOME/Documents"
```
> This affects programs launched from that shell. Prefer system library configuration for permanent system-wide paths.
>
> **Caution** : Avoid changing LD_LIBRARY_PATH globally or adding untrusted library directories; doing so can cause compatibility or security problems.
