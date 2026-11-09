# Debian Package Management
## APT Configuration
APT repository configuration is mainly stored under:
```bash
cd /etc/apt/
ls
cat sources.list
```
Additional repository configuration can be stored in:
```bash
cd /etc/apt/sources.list.d/
ls
cat <repository-file>
```

## dpkg
`dpkg` is the low-level package management tool used to install and manage `.deb` packages.
### Install a Local Package
```bash
dpkg -i package.deb
```
Example:
```bash
dpkg -i webmin-2.021-all.deb
```
### Force Installation
```bash
dpkg --force-all -i package.deb
```
> Use force options carefully. They can leave the package system in an inconsistent state.
### List Installed Packages
```bash
dpkg -l
```
Search installed packages:
```bash
dpkg -l | grep firefox
```
### Package Information
```bash
dpkg -s firefox
```
### List Files Installed by a Package
```bash
dpkg -L firefox
```
### Remove a Package
```bash
dpkg -r webmin
```
### Purge a Package
```bash
dpkg -P webmin
```
`-r` removes the package while generally leaving configuration files.
`-P` removes the package and its configuration files.

## APT
APT provides higher-level package management and handles package dependencies.
### Update Package Index
```bash
apt-get update
```
### Upgrade Packages
```bash
apt-get upgrade
```
### Distribution Upgrade
```bash
apt-get dist-upgrade
```
### Install a Package
```bash
apt-get install firefox
```
### Remove a Package
```bash
apt-get remove firefox
```
### Purge a Package
```bash
apt-get purge firefox
```
### Remove Unused Dependencies
```bash
apt-get autoremove
```
Commands can be chained:
```bash
apt-get update && apt-get upgrade
```

## apt-cache
Search package information without installing the package.
```bash
apt-cache search firefox
apt-cache show firefox
apt-cache depends firefox
apt-cache rdepends firefox
```
- `search` → search available packages
- `show` → package information
- `depends` → dependencies
- `rdepends` → reverse dependencies

## Downloading Packages for Offline Installation
### Download Only
```bash
apt download nginx
```
This downloads the `.deb` package without installing it.
### Using apt-offline
`apt-offline` can be used for systems with limited or no network access.
Example workflow:
```bash
apt-offline set nginx.sig --install-packages nginx
apt-offline get nginx.sig --bundle nginx.zip
apt-offline install nginx.zip
```
This separates dependency/package retrieval from installation on the offline system.

## Package Configuration
Some installed packages provide configuration through:
```bash
dpkg-reconfigure postfix
```

## Dynamic Linker Cache
```bash
ldconfig
```
`ldconfig` updates the system's shared-library cache and symbolic links.

## dpkg vs APT
|Tool|	Role|
|:---|:---:|
|dpkg|	Low-level .deb package management|
|apt / apt-get|	High-level package management and dependency handling|
|apt-cache|	Query package information|
|apt-offline|	Transfer package/dependency information for offline systems|
