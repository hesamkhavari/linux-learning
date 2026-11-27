# System Time and NTP Synchronization

### Linux uses two clocks :
- **System Clock** : The clock maintained by the Linux kernel while the system is running.

- **Hardware Clock (RTC)** : A clock maintained by the motherboard. It continues running when the system is powered off.

## 1. UTC vs Local Time
**UTC (Coordinated Universal Time)** : A global time standard used as a reference.

**Local Time** : The time displayed for a specific region, based on its time zone and daylight saving rules.

> `Servers` commonly keep the hardware clock in `UTC` and use a configured time zone to display local time.

**Important** : Changing the time zone does not change the actual `UTC` time.

## 2. System Clock and Hardware Clock
Useful commands :
```bash
date                  # Display system clock
date -u               # Display system clock in UTC
hwclock               # Display hardware clock
hwclock --show        # Show hardware clock
cat /etc/adjtime      # Show hardware clock settings
```

### Synchronizing the Clocks
```bash
sudo hwclock --systohc --utc
```
Write the system clock to the hardware clock and store it as UTC.

```bash
sudo hwclock --hctosys
```
Set the system clock from the hardware clock.

Short options :
```bash
hwclock -w    # System clock to hardware clock
hwclock -s    # Hardware clock to system clock
```

### Hardware Clock Time Mode
```bash
sudo hwclock --systohc --utc
sudo hwclock --systohc --localtime
```
- `--utc` : Hardware clock stores UTC.

- `--localtime` : Hardware clock stores local time.

> `UTC` is generally recommended for Linux servers.

The `/etc/adjtime` file stores hardware clock adjustment information. Its third line normally indicates `UTC` or `LOCAL`.

## 3. NTP (Network Time Protocol)
`NTP` synchronizes system clocks with remote time servers over a network.

The `NTP Pool` Project provides public time servers :
```bash
https://www.pool.ntp.org/
```
Example pool addresses :
```bash
pool 2.pool.ntp.org
pool 0.asia.pool.ntp.org
pool 2.asia.pool.ntp.org
```
Pool servers are selected by geographic and network considerations, not simply by time zone.

## 4. Modern Time Synchronization

### Modern Linux systems commonly use one of these services :
- `systemd-timesyncd` : A simple network time synchronization service.

- `Chrony` : A modern and commonly used NTP implementation.

- `ntpd` : The traditional NTP daemon.

Check synchronization status :
```bash
timedatectl status
timedatectl timesync-status
```
Enable NTP synchronization when supported :
```bash
sudo timedatectl set-ntp true
```
Disable it :
```bash
sudo timedatectl set-ntp false
```

### Chrony
`Chrony` is commonly used on modern servers.

Install :
```bash
sudo apt install chrony       # Debian / Ubuntu
sudo dnf install chrony       # RHEL-based systems
```
Check service status :
```bash
systemctl status chrony       # Debian / Ubuntu
systemctl status chronyd      # Common on RHEL-based systems
```
Start and enable the service :
```bash
sudo systemctl enable --now chrony
```
On systems using chronyd :
```bash
sudo systemctl enable --now chronyd
```
Useful Chrony commands :
```bash
chronyc tracking
chronyc sources -v
chronyc sourcestats
```
Configuration files :
```bash
/etc/chrony/chrony.conf      # Common on Debian / Ubuntu
/etc/chrony.conf             # Common on RHEL-based systems
```
Example configuration :
```bash
pool 2.pool.ntp.org iburst
```
After changing the configuration, restart the relevant service.

## 5. Traditional NTP Configuration
Older systems may use `ntpd`.

Install :
```bash
sudo apt install ntp
sudo yum install ntp
```
The traditional configuration file is :
```bash
/etc/ntp.conf
```
Example :
```bash
server 2.pool.ntp.org
server 0.asia.pool.ntp.org
server 2.asia.pool.ntp.org
```
Manage the service :
```bash
sudo systemctl start ntpd
sudo systemctl stop ntpd
sudo systemctl restart ntpd
```
The service name depends on the distribution and installed `NTP` implementation.

### ntpdate (Legacy Tool)
Older systems used `ntpdate` to make a one-time clock adjustment :
```bash
ntpdate 2.pool.ntp.org
```
`ntpdate` is deprecated on many modern systems. Prefer `Chrony` or a system time synchronization service.

> Do not run a one-time clock adjustment tool while another NTP service is actively controlling the clock.

## 6. Troubleshooting
Useful commands :
```bash
timedatectl status
date -u
hwclock --show
chronyc tracking
chronyc sources -v
systemctl status chronyd
journalctl -u chronyd
```

### Common problems :
- Incorrect time zone.

- Hardware clock configured as `UTC` on one operating system and `local` time on another.

- NTP service is stopped.

- DNS or network connectivity problems.

- Firewall rules blocking `NTP` traffic **(normally UDP port `123`)**.

- Incorrect or unreachable NTP servers.

### Important Notes
- Linux uses the `system clock` during normal operation.

- The `hardware clock` provides time when the system starts.

- Use `UTC` for hardware clocks on most Linux servers.

- Use `NTP` to keep system time accurate.

- A permanent `NTP` service is better than a `daily cron job`.

- Do not run multiple time synchronization services at the same time unless they are intentionally configured to work together.
