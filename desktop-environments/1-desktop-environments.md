# Linux Desktop Environments

## Debian Package Management

### Update Package Lists

Update the local package index.

```bash
sudo apt update
```
### Upgrade Installed Packages
Upgrade installed packages to their available versions.
```bash
sudo apt upgrade
```
## Installing KDE Plasma
### KDE Plasma Desktop
Install the KDE Plasma desktop environment.
```bash
sudo apt install kde-plasma-desktop
```
### KDE Full
Install the full KDE package set.
```bash
sudo apt install kde-full
```
## Reconfiguring KDE Plasma
### Reconfigure the KDE Plasma desktop package.
```bash
sudo dpkg-reconfigure kde-plasma-desktop
```

### Important Notes
- `apt update` updates the local package index.
- `apt upgrade` upgrades installed packages.
- `kde-plasma-desktop` installs the KDE Plasma desktop environment.
- `kde-full` installs a much larger set of KDE packages.
- `dpkg-reconfigure` can be used to reconfigure an installed package.
