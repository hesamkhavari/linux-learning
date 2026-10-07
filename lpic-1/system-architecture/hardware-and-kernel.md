# Hardware, Kernel Interfaces and Modules
## `/sys` and sysfs

Linux exposes information about devices, buses, drivers and kernel objects through **sysfs**.

The usual mount point is:
```bash
/sys
```
Explore block devices:
```bash
ls /sys/block/
```
Explore system buses:
```bash
cd /sys/bus/
ls
```
USB devices:
```bash
cd /sys/bus/usb/
ls -l
```
PCI devices:
```bash
cd /sys/bus/pci/devices/
ls -l
```
> `/sys` is the mount point; `sysfs` is the filesystem provided by the Linux kernel.

## `/proc` and procfs
`/proc` is a pseudo-filesystem provided by the Linux kernel.

It exposes information about:
* Processes
* CPU
* Memory
* Kernel parameters
* Mounted filesystems
* System state
Examples:
```bash
cd /proc
ls
```
CPU information:
```bash
cat /proc/cpuinfo
```
Mounted filesystems:
```bash
cat /proc/mounts
```
> `/proc` is not a normal disk-based filesystem. Its contents are dynamically provided by the kernel.

## `/proc/sys`
Kernel runtime parameters can be exposed under:
```Plain text
/proc/sys/
```
Example:
```bash
cd /proc/sys/
ls
```
Filesystem-related parameters:
```bash
cd /proc/sys/fs/
ls
```
Network-related parameters:
```bash
cd /proc/sys/net/ipv4/
ls
```

### IP Forwarding
Check the current IPv4 forwarding state:
```bash
cat /proc/sys/net/ipv4/ip_forward
```
Enable it temporarily:
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```
> Changes made through `/proc/sys` or `sysctl -w` are runtime changes and may not survive reboot.

## sysctl
`sysctl` provides a convenient interface for reading and changing kernel parameters.

Show a parameter:
```bash
sysctl net.ipv4.ip_forward
```
Change a parameter temporarily:
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```
Persistent configuration can be placed in:
```Plain text
/etc/sysctl.conf
```
After editing:
```bash
sudo sysctl -p
```
> Be careful when changing kernel parameters. Some values can affect system stability, networking or resource limits.

## Device Files: `/dev`
Linux exposes device nodes under:
```Plain text
/dev
```
Example:
```bash
cd /dev
ls
```
Device files provide an interface between applications and kernel device drivers.
> A device itself is not mounted. Filesystems are mounted; device nodes provide access to devices.

## Hardware Detection
### lspci
List PCI devices:
```bash
lspci
```
### lsusb
List USB devices:
```bash
lsusb
```
### lshw
Display detailed hardware information:
```bash
sudo lshw
```
### lsmod
Display currently loaded kernel modules:
```bash
lsmod
```

## Kernel Modules
Kernel modules can provide drivers or other functionality that can be loaded into the running kernel.
### Load a Module
```bash
sudo modprobe vmxnet3
```
### Remove a Module
```bash
sudo modprobe -r vmxnet3
```
Check loaded modules:
```bash
lsmod
```
### rmmod
A module can also be removed with:
```bash
sudo rmmod vmxnet3
```
> `modprobe -r` is generally preferred because `modprobe` understands module dependencies.

## Module Configuration
Module-related configuration is commonly stored under:
```Plain text
/etc/modprobe.d/
```
Example:
```bash
ls /etc/modprobe.d/
```
Some systems also use:
```Plain text
/etc/modules
```
for modules that should be loaded during system startup.

## udev and `/dev`
Modern Linux systems use udev for dynamic device management.

A simplified relationship is:
```Plain text
Kernel
   ↓
sysfs
   ↓
udev
   ↓
/dev
```
udev receives device events from the kernel and manages device nodes and related rules.

## udev Rules
Custom hardware behavior can be defined using udev rules.

Rules can match device attributes such as:
* Vendor ID
* Product ID
* Device type
* Serial number
* Other device attributes
This allows Linux to apply specific actions when a device is connected.
> udev rules are commonly stored under `/etc/udev/rules.d/`.

## Important Concepts
```Plain text
Hardware
   ↓
Bus (PCIe / USB / etc.)
   ↓
Kernel Driver
   ↓
Kernel Device Model
   ↓
sysfs
   ↓
udev
   ↓
/dev
   ↓
Applications
```
Applications generally do not need to know whether a storage device is connected through SATA, SCSI, USB or another bus. They interact with the appropriate kernel interfaces and device/filesystem layers.

## Important Distinctions
|Component|	Purpose|
|:---|:---:|
|/proc|	Kernel and process information|
|/sys|	Kernel device model and hardware information|
|/dev|	Device nodes|
|udev|	Dynamic device management|
|lspci|	PCI device information|
|lsusb|	USB device information|
|lsmod|	Loaded kernel modules|
|modprobe|	Load/remove modules|
|sysctl|	Read/change kernel parameters|
