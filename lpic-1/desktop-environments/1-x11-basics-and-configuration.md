# X11 Basics and Configuration

## 1. What is X11 ?
X11 (X Window System) is a system that allows graphical applications to display windows and receive user input.

### It uses a Client-Server model :

- `X Server` : Controls the display, keyboard, and mouse. It receives requests from clients.

- `X Client` : A graphical application that requests the X Server to display its interface.

- `Window Manager` : Controls window position, size, borders, and behavior.

- `Display Manager` : Provides the graphical login screen and starts user sessions.

> **Important** : The `X Server` and `X Clients` can run on the same computer or on different computers over a network.

## 2. Font Server
A `Font Server` provides fonts to X applications and the X Server.

It does not process font size or graphics. It provides **access to available fonts**.

Modern Linux systems usually manage fonts **locally** without a separate Font Server.

## 3. X11 Configuration

Modern Linux distributions usually detect graphics cards, monitors, and input devices **automatically**.

The main `Xorg` configuration locations are :
```bash
/etc/X11/xorg.conf
/etc/X11/xorg.conf.d/
```
Check the configuration :
```bash
ls /etc/X11/
ls /etc/X11/xorg.conf.d/
cat /etc/X11/xorg.conf
```

> **Note** : The `xorg.conf` file may not exist. Modern systems often use automatic configuration and small files inside `xorg.conf.d`.

### Important Xorg Sections

Section|Purpose
|---|---|
ServerLayout|Defines the overall layout
Files|Defines file and font paths
Module|Loads Xorg modules
InputDevice|Defines input devices (legacy)
Monitor|Monitor settings
Device|Graphics device settings
Screen|Connects monitor and graphics device

> Not all sections are required.

## 4. Generate Xorg Configuration
The traditional command is :
```bash
sudo Xorg -configure
```
It tries to detect hardware and generate a configuration file, often named `xorg.conf.new`.

**Important** : This is mainly a troubleshooting method. Modern systems usually do not need manual `Xorg` configuration.

Do not replace existing configuration files without checking the generated file first.

## 5. Start the Graphical Environment
Start an X session :
```bash
startx
```
Start another X display :
```bash
startx -- :1
```

- `:0` : Common first display.

- `:1` : Another display.

Check the current display :
```bash
echo "$DISPLAY"
```
The `DISPLAY` variable tells graphical applications which `X Server` to use.

## 6. Boot into Text Mode
On systemd-based systems, the default targets can control whether the system starts in text or graphical mode.

Check the current default :
```bash
systemctl get-default
```
Set text mode as the default :
```bash
sudo systemctl set-default multi-user.target
```
Set graphical mode as the default :
```bash
sudo systemctl set-default graphical.target
```
Switch to text mode now :
```bash
sudo systemctl isolate multi-user.target
```
Switch to graphical mode now :
```bash
sudo systemctl isolate graphical.target
```
Reboot :
```bash
sudo systemctl reboot
```

> **Note** : `init 6` is an older command for rebooting. systemctl reboot is the modern systemd command.

## 7. X11 Access Control
The `xhost` command controls which hosts can connect to the `X Server`.

Show the current access list :
```bash
xhost
```
Allow a specific host :
```bash
xhost +172.20.1.1
```
Remove that host :
```bash
xhost -172.20.1.1
```
Enable access control :
```bash
xhost -
```
Disable access control :
```bash
xhost +
```

> **Security Warning** : `xhost +` allows connections without normal host-based access restrictions. It is unsafe on shared or network-connected systems.

Prefer more secure authentication methods such as `xauth` and `SSH X11 forwarding`.

## 8. Useful X11 Commands

Command|Purpose
|---|---|
`startx`|Start an X session
`xhost`|Manage X Server access
`xauth`|Manage X authentication cookies
`xdpyinfo`|Show X Server information
`xwininfo`|Show information about a window
`xrandr`|Show and manage display settings
`echo $DISPLAY`|Show the current display
`Xorg -configure`|Generate a configuration file

## 9. Remote Graphical Applications with SSH
`X11 forwarding` allows graphical applications on a remote Linux system to appear on the local display.

Connect to a remote system :
```bash
ssh -X username@server
```
Run a graphical application remotely :
```bash
firefox
```
The application runs on the remote computer, but its graphical window appears on the local computer.

### Trusted X11 Forwarding
```bash
ssh -Y username@server
```
The application runs on the remote computer, but its graphical window appears on the local computer.

- `-X` : Enables restricted X11 forwarding.

- `-Y` : Enables trusted X11 forwarding.

> Use `-Y` only when you trust the remote system.

### Important :

- The local computer needs a working X Server or compatible graphical environment.

- The remote `SSH` server must allow X11 forwarding.

- `xauth` is commonly required on the remote system.

- `SSH` normally sets `DISPLAY` automatically. Do not set it manually when using SSH forwarding.

## 10. Troubleshooting
Check X Server information :
```bash
xdpyinfo
```
Check display windows :
```bash
xwininfo
```
Check the display variable :
```bash
echo "$DISPLAY"
```
Check `Xorg` logs (if available):
```bash
grep -E '\(EE\)|\(WW\)' /var/log/Xorg.0.log
```
Check system logs :
```bash
journalctl -b | grep -i xorg
```

Common problems :
- Can't open display: Check `DISPLAY` and `X Server` access.

- X11 forwarding request failed: Check `SSH` server settings and `xauth`.

- Graphical session does not start: Check `Xorg` logs and graphics drivers.

## 11. Important Notes
- `X11` is a display protocol, not a desktop environment.

- `X Server` manages display and input devices.

- `X Clients` are graphical applications.

- `Xorg` is a common implementation of the X11 system.

- Modern systems often configure `Xorg` automatically.

- `Wayland` is a newer display protocol used by many modern Linux distributions.

- `X11` applications can run remotely through SSH forwarding.

- Avoid using `xhost +` because it weakens access control.
