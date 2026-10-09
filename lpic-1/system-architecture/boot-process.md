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
