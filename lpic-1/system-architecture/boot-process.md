# Linux Boot Process and GRUB
## 1. Overview of the Boot Process
A typical Linux boot process follows these stages:
```Plain text
Power On
 ↓
BIOS / UEFI
 ↓
GRUB Bootloader
 ↓
Linux Kernel + initramfs
 ↓
PID 1 (usually systemd)
 ↓
System Services and Targets
 ↓
Login / Desktop
```
* **BIOS/UEFI**: Initializes hardware and selects a boot device.
* **GRUB**: Loads the selected kernel and initramfs.
* **Kernel**: Initializes core operating-system components and detects hardware.
* **initramfs**: Provides a temporary environment needed to access the real root filesystem.
* **PID 1**: The first userspace process, usually `systemd` on modern distributions.
* **Services and targets**: Bring the system to its configured operating state.
> The exact sequence depends on the distribution and system configuration.

## 2. GRUB Bootloader
GRUB allows the user to select an operating system or kernel entry and can provide recovery and troubleshooting options.
### Display the GRUB Menu
Depending on the system configuration, the GRUB menu may be displayed by:
* Holding `Shift` during startup on some BIOS-based systems.
* Pressing `Esc` during startup on some UEFI-based systems.
> The menu may also be configured to appear automatically or remain hidden.
### Edit a Boot Entry
1. Select a GRUB entry.
2. Press `e` to edit it.
3. Inspect the boot commands.
4. Make temporary changes if needed.
5. Press `Ctrl+X` or `F10` to attempt booting with the edited entry.
> Changes made through this editor are normally temporary and do not permanently modify the GRUB configuration.
### Common GRUB Entries
Example of a simplified boot entry:
```Plain text
set root='(hd0,1)'
search --no-floppy --fs-uuid --set=root <filesystem-uuid>
linux /boot/vmlinuz-<version> root=UUID=<root-uuid> ro quiet splash
initrd /boot/initrd.img-<version>
```
* These are illustrative examples. Actual entries, paths, UUIDs and parameters vary by distribution and installation.
* `set root`: Specifies a GRUB device or filesystem context.
* `search --fs-uuid`: Searches for a filesystem by UUID.
* `linux`: Loads the Linux kernel and passes kernel command-line parameters.
* `initrd`: Loads the initial RAM filesystem image.
### Display Boot Messages
The parameters `quiet` and `splash` can reduce visible boot messages or display a graphical startup screen.
Removing these parameters from the temporary GRUB editor entry may reveal more startup messages.
> Do not permanently edit GRUB configuration files unless you understand the change and have a recovery plan.

## 3. Inspect `/boot`
The `/boot` directory commonly contains kernel images, initramfs images and bootloader-related files.
```bash
cd /boot
ls -lh
```
Look for GRUB files:
```bash
ls /boot/grub/
```
> Some distributions use a different path, such as `/boot/grub2/`.
List kernel images:
```bash
ls /boot/vmlinuz*
```
List initramfs images where applicable:
```bash
ls /boot/initrd*
ls /boot/initramfs*
```
The filenames and available patterns depend on the distribution.

## 4. Kernel Messages with dmesg
`dmesg` displays messages from the kernel's ring buffer. These messages can help diagnose hardware detection, drivers and kernel initialization.
Display messages:
```bash
dmesg
```
Display readable timestamps on supported systems:
```bash
dmesg -T
```
Search for errors:
```bash
dmesg --level=err,warn
```
Search for hardware-related messages:
```bash
dmesg | grep -i usb
dmesg | grep -i firmware
dmesg | grep -i error
```
> `dmesg` is not the same as a persistent log file. Its output may be limited to messages currently retained in the kernel ring buffer.

## 5. Boot Logs and journalctl
### Inspect Traditional Log Files
Some systems provide a kernel log file:
```bash
cat /var/log/dmesg
```
Some distributions also use:
```bash
cat /var/log/messages
```
A rotated log may have a name such as:
```bash
cat /var/log/messages.1
```
These files are not guaranteed to exist on every distribution. Modern systems may rely primarily on the systemd journal.
### Inspect the Current Boot
Show logs from the current boot:
```bash
journalctl -b
```
Show kernel messages from the current boot:
```bash
journalctl -k -b
```
Show warnings and errors from the current boot:
```bash
journalctl -b -p warning
```
Show only errors from the current boot:
```bash
journalctl -b -p err
```
List recorded boots:
```bash
journalctl --list-boots
```
Inspect the previous boot, if its logs are retained:
```bash
journalctl -b -1
```
Follow new journal messages:
```bash
journalctl -f
```

## 6. Process Tree with pstree
`pstree` displays processes in a tree structure, helping show parent-child relationships.
Display the process tree:
```bash
pstree
```
Include process IDs:
```bash
pstree -p
```
Inspect PID 1 and its descendants:
```bash
pstree -p 1
```
On many modern Linux systems, `systemd` is PID 1 and starts or supervises system services.
If `pstree` is unavailable, the `ps` command can also show process relationships:
```bash
ps -ef --forest
```

## 7. Boot Troubleshooting Workflow
When a Linux system has boot problems:
1. **GRUB**: Is the expected boot entry available?
2. **Kernel**: Are kernel messages showing driver or hardware errors?
3. **Root filesystem**: Can the system locate and mount the root filesystem?
4. **Userspace**: Does PID 1 start successfully?
5. **Services**: Which service or target failed?
Useful commands after the system starts:
```bash
uname -r
dmesg --level=err,warn
journalctl -b -p warning
systemctl --failed
systemctl get-default
```
`uname -r` shows the running kernel version. `systemctl --failed` lists failed units, while `systemctl get-default` displays the configured default target.

## Key Distinctions
|Component	|Main purpose|
|BIOS / UEFI	|Hardware initialization and boot-device selection|
|GRUB	|Selects and loads the kernel|
|Kernel	|Core operating-system functions and hardware management|
|initramfs	|Temporary early-userspace environment|
|systemd / PID 1	|Starts and supervises userspace services|
|`dmesg`	|Kernel ring-buffer messages|
|`journalctl`	|Reads the systemd journal|
|`pstree`	|Displays process relationships|
