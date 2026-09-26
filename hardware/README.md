# Linux Hardware Management

## System Information

### uname

Display basic system information.

```bash
uname
uname -a
```
## CPU Information
### lscpu
Display detailed CPU architecture and configuration information.
```bash
lscpu
```
### /proc/cpuinfo
View detailed CPU information exposed by the Linux kernel.
```bash
cat /proc/cpuinfo
```
## PCI Devices
### lspci
List devices connected through the PCI bus.
```bash
lspci
```
Useful for identifying hardware such as network cards, GPUs, and other PCI devices.
## Block Devices
### lsblk
Display block devices such as disks and partitions.
```bash
lsblk -f
```
The `-f` option also displays filesystem information.
## Disk Partitioning
### fdisk
Interactive tool for managing disk partition tables.
```bash
fdisk
```
### fdisk -l
List available disks and their partition tables.
```bash
fdisk -l
```
Be careful when using `fdisk` because modifying a partition table can cause data loss.
## Graphical Partition Manager
### GParted
Graphical tool for viewing and managing disk partitions.
```bash
gparted
```
## X11 Configuration
Some Linux systems store X11-related configuration under:
```bash
cd /etc/X11
ls
```
 The presence and contents of this directory depend on the Linux distribution and graphical environment.
