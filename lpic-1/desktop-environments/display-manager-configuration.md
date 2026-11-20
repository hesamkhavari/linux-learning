# Display Manager Configuration

## 1. What is a Display Manager?
A Display Manager (DM) is a program that provides a graphical login screen.

It allows users to :
- Log in to a graphical session.

- Select a desktop environment.

- Start a graphical session.

**Important** : A `Display Manager` is not the same as a `Desktop Environment`.

## 2. Common Display Managers

Display Manager|Description
|---|---|
GDM|Used by GNOME
SDDM|Commonly used by KDE Plasma
LightDM|Lightweight and flexible
XDM|Basic and older Display Manager
KDM|Older KDE Display Manager; mostly obsolete

## 3. Check Desktop Environment
Show the current desktop session :
```bash
echo "$DESKTOP_SESSION"
```
Check the session type :
```bash
echo "$XDG_SESSION_TYPE"
```
### Possible values :
- `x11` : X11 session

- `wayland` : Wayland session

List available `X11` sessions :
```bash
ls /usr/share/xsessions/
```
List available `Wayland` sessions :
```bash
ls /usr/share/wayland-sessions/
```

## 4. Install Display Managers
### Ubuntu / Debian
Update package information :
```bash
sudo apt update
```
Install GDM :
```bash
sudo apt install gdm3
```
Other options :
```bash
sudo apt install sddm
sudo apt install lightdm
sudo apt install xdm
```
> **Note** : Install the Display Manager you want to use, not all of them.

### CentOS / RHEL
Older CentOS versions use `YUM` :
```bash
sudo yum groupinstall "X Window System" "KDE"
```
Modern RHEL-based systems use `DNF` :
```bash
sudo dnf group list --available
```
Install a graphical environment group when needed :
```bash
sudo dnf group install "Server with GUI"
```
The available groups and package names depend on the distribution version and repositories.

## 5. Check Display Manager
Check the Display Manager service :
```bash
systemctl status display-manager
```
Check its service link :
```bash
ls -l /etc/systemd/system/display-manager.service
```
On Debian and Ubuntu, check the configured Display Manager :
```bash
cat /etc/X11/default-display-manager
```
Example output :
```bash
/usr/sbin/gdm3
```
> **Note** : `/etc/X11/default-display-manager` is mainly used by Debian-based distributions. It is not a universal Linux configuration file.

## 6. Select the Default Display Manager
On Debian and Ubuntu, use :
```bash
sudo dpkg-reconfigure gdm3
```
If multiple Display Managers are installed, the configuration tool may allow you to select the default one.

The available options depend on installed packages.

## 7. Runlevels and Systemd Targets
Traditional Linux runlevels :

Runlevel|Meaning
|---|---|
0|Shutdown
1|Single-user / Rescue mode
2|Multi-user mode (varies by distribution)
**3**|Multi-user text mode
4|Custom / Unused
**5**|Graphical mode
6|Reboot

Modern Linux systems use `systemd` targets.

Systemd Target|Traditional Runlevel
|---|---|
poweroff.target|0
rescue.target|1
multi-user.target|3
graphical.target|5
reboot.target|6

> Important: This is a traditional mapping. **Runlevels 2 and 4** can have different meanings across distributions.

## 8. Check and Manage Systemd Targets
Show the default target :
```bash
systemctl get-default
```
List active targets :
```bash
systemctl list-units --type=target
```
Set text mode as the default :
```bash
sudo systemctl set-default multi-user.target
```
Set graphical mode as the default :
```bash
sudo systemctl set-default graphical.target
```
Switch to text mode immediately :
```bash
sudo systemctl isolate multi-user.target
```
Switch to graphical mode immediately :
```bash
sudo systemctl isolate graphical.target
```
Reboot :
```bash
sudo systemctl reboot
```

## 9. Start and Stop Graphical Services
Start the Display Manager :
```bash
sudo systemctl start display-manager
```
Stop it :
```bash
sudo systemctl stop display-manager
```
Restart it :
```bash
sudo systemctl restart display-manager
```
> **Warning** : Stopping or restarting the Display Manager can close active graphical sessions and cause users to lose unsaved work.

## 10. Start X Manually
Start an X session from a text terminal :
```bash
startx
```

> Note: `startx` starts an X session. It is not a Display Manager and does not provide a graphical login screen.

## 11. Important Notes

- `GDM`, `SDDM`, `LightDM`, and `XDM` are Display Managers.

- A `Desktop Environment` provides the graphical desktop and applications.

- `graphical.target` starts the graphical system target.

- `multi-user.target` starts a multi-user system without the normal graphical target.

- `systemctl set-default` changes the default target for future boots.

- `systemctl isolate` switches to another target immediately.

- `startx` starts an X session from a terminal.

- Modern Linux systems use `systemd` instead of traditional `SysV init` in most cases.

- `GRUB` configuration is usually not needed just to switch between text and graphical boot modes.
