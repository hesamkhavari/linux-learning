# Linux Logging
## /var/log
System and application logs are commonly stored under:
```bash
cd /var/log/
ls
```
Examples of log files on Debian/Ubuntu systems include:
```bash
cat auth.log
cat boot.log
cat syslog
```
> Available log files depend on the distribution and logging configuration.

## Searching Logs
Instead of reading a large log file completely, search for specific information:
```
grep dhcp /var/log/syslog
```

## Kernel Messages
```bash
dmesg
```
Search kernel messages:
```bash
dmesg | grep eth0
```
View the beginning:
```bash
dmesg | head
```
View the end:
```bash
dmesg | tail
```
`dmesg` is useful for investigating kernel and hardware-related events.

## CUPS Logs
CUPS logs may be stored under:
```bash
cd /var/log/cups/
ls -al
```
## Important Note
Log locations and filenames vary between distributions and configurations. Modern Linux systems may also use `systemd-journald` and `journalctl` for centralized logging.
