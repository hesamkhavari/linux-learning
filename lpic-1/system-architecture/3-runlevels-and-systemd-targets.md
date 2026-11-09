# Linux Runlevels and systemd Targets

## 1. What Is a Runlevel?
A runlevel is a traditional SysV init concept that represents a particular operating state of a Linux system.
Runlevels were associated with sets of startup and shutdown scripts used to manage services.
Modern Linux distributions commonly use systemd targets instead of traditional runlevels.

## 2. Traditional Runlevels
Runlevel|Traditional purpose|Common systemd equivalent
|:---|:---:|:---:|
0|Halt / power off|poweroff.target
1|Single-user / rescue mode|rescue.target
2|Multi-user mode|multi-user.target
3|Multi-user mode with networking|multi-user.target
4|Custom or undefined, depending on distribution|multi-user.target commonly
5|Graphical multi-user mode|graphical.target
6|Reboot|reboot.target
>Runlevel-to-target mappings are compatibility conventions, not a universal one-to-one mapping.
>
>Exact behavior depends on the distribution and configuration.
### Important Notes
- Traditional SysV runlevels 2, 3 and 4 may have `different meanings` depending on the distribution.
- In many Debian-based systems, runlevels 2–5 traditionally provide `multi-user` operation, with graphical services depending on configuration.
- In traditional Red Hat configurations, runlevel 3 commonly represents `multi-user text mode` and runlevel 5 `graphical mode`.
- Modern Debian and Red Hat-family distributions commonly use `systemd`, so historical default-runlevel assumptions `should not be applied automatically`.
- 
## 3. systemd Targets
A target `groups units` to bring the system into a `particular state`. Targets can depend on other units, including services and other targets.
### Common Targets
Target|Purpose
|:---|:---:|
poweroff.target|Shut down the system
rescue.target|Enter a minimal rescue environment
multi-user.target|Multi-user, non-graphical operation
graphical.target|Graphical multi-user operation
reboot.target|Reboot the system
emergency.target|Minimal emergency environment
### Check the Default Target
```bash
systemctl get-default
```
### Set the Default Target
Set text-based multi-user operation as the default:
```bash
sudo systemctl set-default multi-user.target
```
Set graphical operation as the default:
```bash
sudo systemctl set-default graphical.target
```
> These commands change the default target for `subsequent boots`. They do not necessarily switch the current session immediately.

### Switch Targets at Runtime
Switch to multi-user mode:
```bash
sudo systemctl isolate multi-user.target
```
Switch to graphical mode:
```bash
sudo systemctl isolate graphical.target
```
> `isolate` can *stop units that are not required by the selected target*.
> 
> Use it carefully, *especially on remote systems*, because it may interrupt services or sessions.
### Inspect Target Information
```bash
systemctl list-units --type=target
```
List installed target unit files:
```bash
systemctl list-unit-files --type=target
```
Inspect a target:
```bash
systemctl status multi-user.target
```

## 4. SysV Init Scripts and /etc/rc*.d/
Traditional SysV `init systems` commonly use the following directories:
```Plain text
/etc/init.d/
/etc/rc0.d/
/etc/rc1.d/
/etc/rc2.d/
/etc/rc3.d/
/etc/rc4.d/
/etc/rc5.d/
/etc/rc6.d/
```
> **/etc/init.d/** contains service scripts `on systems that use this layout`.
>
> **/etc/rcN.d/** contains `links` that determine `which scripts` are started or stopped for a particular runlevel.

These paths may be absent or compatibility-managed on modern systems.
### Script Naming Convention
Example names:
```Plain text
S20network
K10service
```
- `S` means start a service when entering the relevant runlevel.
- `K` means kill/stop a service when leaving or processing that runlevel.
- The number helps determine the ordering of scripts.

The exact ordering and execution behavior depend on the SysV init implementation and its configuration.

### Inspect the Directories
```bash
ls -la /etc/init.d/
ls -la /etc/rc1.d/
ls -la /etc/rc2.d/
ls -la /etc/rc6.d/
```
Inspect links:
```bash
ls -l /etc/rc2.d/
```
A typical SysV layout uses symbolic links from the runlevel directories to scripts under `/etc/init.d/`.

Do not create or delete service links manually unless you understand the distribution's init configuration. 

Modern systems generally use systemd unit files and targets.

## 5. Managing Services
### Modern systemd Method
Check a service:
```bash
systemctl status NetworkManager
```
Start it:
```bash
sudo systemctl start NetworkManager
```
Stop it:
```bash
sudo systemctl stop NetworkManager
```
Restart it:
```bash
sudo systemctl restart NetworkManager
```
Enable it at boot:
```bash
sudo systemctl enable NetworkManager
```
Disable automatic startup:
```bash
sudo systemctl disable NetworkManager
```
Enable and start it immediately:
```bash
sudo systemctl enable --now NetworkManager
```
> Service names differ between distributions. Check the actual `unit name` before running commands.

### Legacy SysV Method
On systems that provide SysV service scripts:
```bash
sudo service <service-name> status
sudo service <service-name> start
sudo service <service-name> stop
```
Direct invocation may also be supported:
```bash
sudo /etc/init.d/<service-name> status
```
These are legacy interfaces. Prefer `systemctl` on systemd-based systems unless compatibility behavior is specifically being studied.

## 6. init and telinit
### Traditional systems used init and telinit to change runlevels.
Examples of historical commands:
```bash
init 1
init 5
init 6
init 0
```
Their traditional meanings are:
- init 1: switch to single-user/rescue mode.
- init 5: switch to graphical runlevel on systems that define it that way.
- init 6: reboot.
- init 0: halt or power off.

> On `systemd-based` systems, these commands may be compatibility interfaces, and behavior can vary.

Modern equivalents include:
```bash
sudo systemctl isolate rescue.target
sudo systemctl isolate graphical.target
sudo systemctl reboot
sudo systemctl poweroff
```
> Do not experiment with init 0, init 6 or target isolation on your main machine.
> 
> They can interrupt your session or shut down the system. Use a VM and save your work first.

## 7. Shutdown and Reboot
### Power Off
```bash
sudo systemctl poweroff
```
Legacy equivalent:
```bash
sudo shutdown -P now
```
### Reboot
```bash
sudo systemctl reboot
```
Legacy equivalent:
```bash
sudo shutdown -r now
```
### Schedule a Shutdown
Shut down in five minutes:
```bash
sudo shutdown -P +5
```
Cancel a scheduled shutdown:
```bash
sudo shutdown -c
```
Send a warning without actually shutting down:
```bash
sudo shutdown -k +5
```
> The `-k` option sends warning messages without performing the shutdown.

Check available options:
```bash
shutdown --help
```

## 8. TTYs and Runlevels
A TTY is a terminal interface. 
`Linux virtual consoles` may be available through key combinations such as `Ctrl+Alt+F1` through `Ctrl+Alt+F6`, depending on the distribution and desktop environment.

TTY numbers are not runlevel numbers.

On `systemd-based` systems, console login services may be managed by getty units.

Inspect them with:
```bash
systemctl list-units 'getty*'
```
Check the current terminal:
```bash
tty
```
### Key Distinctions
Concept|Meaning
|:---|:---:|
Runlevel|Traditional SysV operating state
Target|systemd unit grouping representing a system state
/etc/init.d/|Legacy service scripts
/etc/rc*.d/|Legacy runlevel startup/stop links
systemctl|Main systemd management command
TTY|Terminal interface, not a runlevel

## Practical Recommendation
For modern Linux administration, learn `systemctl` and `systemd` targets first, while understanding SysV `runlevels` and `startup scripts` for compatibility and exam purposes.
