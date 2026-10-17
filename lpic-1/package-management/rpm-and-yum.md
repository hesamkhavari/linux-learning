# RPM, YUM, DNF and DNF5 Package Management

## 1. Package Management Tools
- `RPM` : Installs, removes, queries, and verifies local RPM packages. It does not automatically resolve dependencies by downloading missing packages.

- `YUM` : Older high-level package manager for RPM-based distributions.

- `DNF` : Modern package manager used by current RHEL-family distributions and Fedora versions that ship DNF.

- `DNF5` : Newer implementation of DNF used by some modern distributions and releases. Availability and command support depend on the distribution.

> **Rule of thumb** : Prefer `dnf` or `dnf5` for normal package management. Learn `yum` for legacy compatibility and `rpm` for low-level package operations.

## 2. `DNF`: Installing, Removing and Updating Packages
Show help:
```bash
dnf --help
dnf install --help
```
Install one or more packages:
```bash
sudo dnf install firefox
sudo dnf install openssh-clients
```
Remove a package:
```bash
sudo dnf remove firefox
```
Reinstall a package:
```bash
sudo dnf reinstall openssh-clients
```
Search for packages:
```bash
dnf search openssh
```
Display package information:
```bash
dnf info openssh-clients
```
List installed packages:
```bash
dnf list --installed
```
List available updates:
```bash
dnf check-update
```
Upgrade installed packages:
```bash
sudo dnf upgrade
```
Update package metadata/cache:
```bash
sudo dnf makecache
```
Clean cached metadata and package data:
```bash
sudo dnf clean all
```
> `dnf update` is generally an alias for `dnf upgrade` in standard DNF implementations.

## 3. Repositories
### Repositories provide package files and metadata.
List enabled repositories:
```bash
dnf repolist
```
List enabled and disabled repositories:
```bash
dnf repolist --all
```
Show repository details:
```bash
dnf repoinfo
```
Repository configuration files:
```bash
ls -l /etc/yum.repos.d/
```
Main DNF configuration:
```bash
cat /etc/dnf/dnf.conf
```

Important paths:
- `/etc/yum.repos.d/*.repo` — repository configuration files.

- `/etc/dnf/dnf.conf` — main DNF configuration.

- `/etc/yum.conf` — legacy YUM configuration path; may be present as a compatibility configuration on some systems.

> Inspect repository files before editing them. Avoid replacing repository definitions with unverified content.

## 4. Finding Packages and Their Files
Search package names and summaries:
```bash
dnf search ssh
```
Search for a package that provides a command or file:
```bash
dnf provides /usr/bin/ssh
```
Query package versions available in repositories:
```bash
dnf repoquery openssh-clients
```
Show files installed by an RPM package:
```bash
rpm -ql openssh-clients
```
Find which installed package owns a file:
```bash
rpm -qf /usr/bin/ssh
```
Show detailed information about an installed package:
```bash
rpm -qi openssh-clients
```
Show documentation files belonging to a package:
```bash
rpm -qd openssh-clients
```

> The path in `dnf provides` must match the file or command you are trying to locate. Package names and file paths can differ across distributions.

## 5. Downloading Packages Without Installing Them
### On systems with the relevant DNF download plugin installed:
Download a package:
```bash
dnf download openssh-clients
```
Download a package and its dependencies:
```bash
dnf download --resolve openssh-clients
```
On older **YUM-based** systems, `yumdownloader` may be available through yum-utils:
```bash
yumdownloader openssh-clients
yumdownloader --resolve openssh-clients
```

> Plugin availability and exact command syntax depend on the installed **DNF version**.
> 
> For **DNF5**, consult `dnf5 download --help` and install the required plugin if necessary.

## 6. RPM: Local Package Operations
### `RPM` is useful for examining and verifying packages, but prefer `DNF` for normal installation and removal.
Install a local RPM file:
```bash
sudo rpm -ivh package.rpm
```
Upgrade or install a package from a local RPM file:
```bash
sudo rpm -Uvh package.rpm
```
Remove an installed package by package name:
```bash
sudo rpm -e package-name
```
Query whether a package is installed:
```bash
rpm -q openssh-clients
```
Show detailed package information:
```bash
rpm -qi openssh-clients
```
List files belonging to a package:
```bash
rpm -ql openssh-clients
```
List documentation files:
```bash
rpm -qd openssh-clients
```
List all installed packages:
```bash
rpm -qa
```
Search installed packages by name:
```bash
rpm -qa | grep -i ssh
```
Verify package files against RPM database records:
```bash
rpm -V openssh-clients
```
Check package signature and digest:
```bash
rpm -K package.rpm
```

### Notes:
- `-i` installs; `-U` upgrades or installs; `-e` erases an installed package.

- `-v` and `-h` enable verbose output and a progress display, respectively.

- `rpm -V` checks package files against recorded attributes; no output usually means no discrepancies were found.

rpm -K checks package digests and signatures where applicable. A successful digest check alone does not establish that the package publisher is trusted.

rpm -Vv adds verbose verification output.

The correct command is rpm -q, not rom -q.
