# Debian Package Management

## 1. Package Management Tools
- **dpkg**: Installs, removes and queries local Debian packages (.deb); it does not resolve repository dependencies automatically.

- **apt-get**: Downloads and manages packages from configured repositories, including dependency resolution.

- **apt-cache**: Queries package metadata and dependency information.

- **aptitude**: Alternative package-management interface with dependency-resolution features.

- **Repository**: A source of packages and metadata; mirrors provide replicated copies of repositories.

## 2. dpkg Commands
Install a local package:
```bash
sudo dpkg -i package.deb
```
List installed packages:
```bash
dpkg -l
dpkg -l 'firefox*'
```
Show package information:
```bash
dpkg -p firefox
```
Find which installed package owns a file:
```bash
dpkg -S /bin/ls
```
Remove a package but keep configuration files:
```bash
sudo dpkg -r package-name
```
Purge package configuration files:
```bash
sudo dpkg -P package-name
```
Reconfigure an installed package:
```bash
sudo dpkg-reconfigure postfix
```

## 3. APT Commands
Refresh repository metadata:
```bash
sudo apt-get update
```
Upgrade installed packages:
```bash
sudo apt-get upgrade
```
Install or remove packages:
```bash
sudo apt-get install package-name
sudo apt-get remove package-name
sudo apt-get purge package-name
```
Remove automatically installed dependencies that are no longer needed:
```bash
sudo apt-get autoremove
```

## 4. Query Package Information
Search for packages:
```bash
apt-cache search firefox
```
Show package details:
```bash
apt-cache show firefox
```
Show dependencies and reverse dependencies:
```bash
apt-cache depends firefox
apt-cache rdepends firefox
```
Inspect package versions and repository sources:
```bash
apt-cache policy firefox
```

## 5. Repositories
APT repository configuration may be stored in:
```bash
/etc/apt/sources.list
/etc/apt/sources.list.d/
```
> Modern Debian systems may also use `.sources` files in `sources.list.d`.

After changing repository configuration, refresh metadata:
```bash
sudo apt-get update
```

## 6. Important Distinctions
- `update` refreshes package metadata; it does not upgrade installed packages.

- `upgrade` installs available upgrades without removing installed packages to resolve dependencies.

- `remove` generally keeps system-wide package configuration files.

- `purge` also removes package configuration files managed by the package manager.

- `dpkg -i` may leave dependencies unresolved; APT is generally preferred when installing from repositories.

> **Caution** : Review `package-manager` prompts before confirming removals or upgrades, especially on `servers`.
