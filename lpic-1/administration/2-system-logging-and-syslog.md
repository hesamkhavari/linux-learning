# System Logging and Syslog

## 1. What Is a Log?
A log is a record of events that happen on a system.

### Why do we need logs?
- `Troubleshooting` system problems.

- `Monitoring` services and applications.

- `Security` auditing and incident investigation.

- `Tracking` system activity.

## 2. Syslog and Logging Services
Service|Description
|---|---|
`syslog`|Traditional logging protocol and older logging implementations
`rsyslog`|Modern logging service with UDP, TCP, and TLS support
`syslog-ng`|Another advanced logging service with filtering and remote logging
`journald`|Systemd service that collects and stores system logs

> `rsyslog` and `syslog-ng` are both widely used. Neither is always better; the choice depends on system requirements.

### Main Syslog Components
- `Sender` : Sends log messages.

- `Receiver` : Receives log messages.

- `Relay` : Forwards messages to another logging server.

- `Message` : Contains information about an event.

> Traditional Syslog commonly uses `UDP` port `514`. Modern implementations can also use `TCP` and `encrypted transport`.

## 3. Syslog Message Format
A traditional syslog rule contains three main parts :
```bash
FACILITY.PRIORITY    ACTION
```
- `Facility` : Identifies the source or category of a message.

- `Priority` : Defines the message severity.

- `Action` : Defines where the message is sent or stored.

Example :
```bash
auth.warning    /var/log/auth.log
```
This rule sends authentication warnings to a log file.

### Facility Codes

Code|Facility|Description
|---|---|---|
0|kern|Kernel messages
1|user|User-level messages
2|mail|Mail system
3|daemon|System daemons
4|auth|Security and authorization
5|syslog|Internal syslog messages
6|lpr|Printing system
7|news|Network news
8|uucp|UUCP subsystem
9|cron|Scheduling services
10|authpriv|Private authentication messages
11|ftp|FTP service
12|ntp|NTP subsystem
13|security|Security messages (legacy alias)
14|console|Console messages
15|cron|Clock daemon (legacy name)
16-23|local0-local7|Custom local facilities

### Priority Levels
Lower numbers mean higher severity.

Code|Keyword|Description
|---|---|---|
0|emerg|System is unusable
1|alert|Immediate action required
2|crit|Critical condition
3|err|Error condition
4|warning|Warning condition
5|notice|Normal but significant event
6|info|Informational message
7|debug|Debugging information

### Useful priority selectors :

```bash
*.*
```
All facilities and priorities.
```bash
*.err
```
All facilities with error severity and higher.
```bash
authpriv.*
```
All authentication-private messages.
```bash
mail.info
```
Mail messages with info severity and higher.
```bash
*.=info
```
Only messages with exactly the info priority.

## 4. Syslog Actions
An action defines where messages are sent.

### Common actions include :
Action|Purpose
|---|---|
File path|Save messages in a file
User name|Send messages to a logged-in user
|*|Send messages to all logged-in users
Named pipe|Send messages to a pipe
Remote host|Forward messages to another server

## 5. Rsyslog Configuration
Main configuration file :
```bash
/etc/rsyslog.conf
```
Additional configuration files :
```bash
/etc/rsyslog.d/
```

### Configure a Remote Log Server
The following examples use modern Rsyslog syntax.

UDP server :
```bash
module(load="imudp")
input(type="imudp" port="514")
```
TCP server :
```bash
module(load="imtcp")
input(type="imtcp" port="514")
```
After configuration :
```bash
sudo systemctl restart rsyslog
sudo systemctl status rsyslog
```
Allow the required `port` through the `firewall`. UDP and TCP are separate protocols.

### Configure a Client
To forward all logs to a remote server :
```bash
*.* action(
    type="omfwd"
    target="172.16.1.107"
    port="514"
    protocol="tcp"
)
```
For UDP, change :
```bash
protocol="udp"
```
Restart the client service :
```bash
sudo systemctl restart rsyslog
sudo systemctl status rsyslog
```

### Legacy Rsyslog Forwarding Syntax
Older configurations may use :
```bash
*.*    @172.16.1.107:514
```

- @ means UDP.

- @@ means TCP.

Example :
```bash
*.*    @@172.16.1.107:514
```

> **Security note** : `TCP` does not automatically encrypt or authenticate syslog traffic. Use `TLS` when logs travel across untrusted networks.

## 6. Viewing Logs
Common log locations depend on the Linux distribution :

File|Purpose
|---|---|
`/var/log/syslog`|General system logs (Debian / Ubuntu)
`/var/log/messages`|General system logs (RHEL-based systems)
`/var/log/auth.log`|Authentication logs (Debian / Ubuntu)
`/var/log/secure`|Authentication logs (RHEL-based systems)
`/var/log/kern.log`|Kernel logs on some systems

Useful commands :
```bash
cd /var/log
ls -lh
tail -f /var/log/syslog
tail -f /var/log/messages
```
With systemd :
```bash
journalctl
journalctl -f
journalctl -k
journalctl -u ssh
journalctl --since today
```
## 7. Klogd and Kernel Logging
`klogd` is a traditional daemon that reads kernel messages and forwards them to the logging system.

On modern Linux systems, kernel messages are commonly available through the kernel log buffer and systemd journal.

### Useful commands :
```bash
dmesg
dmesg -T
journalctl -k
```
`klogd` is mainly relevant to older Linux systems.

## 8. Logger Command
`logger` sends a message to the system logging service.

Basic usage :
```bash
logger "System test message"
logger "Service started"
```
Set a facility and priority :
```bash
logger -p user.info "Application started"
logger -p auth.warning "Authentication warning"
```
Set a tag :
```bash
logger -t MyApp "Application event"
```
Send a message to a specific system logger socket when needed :
```bash
logger -p user.info -t MyApp "Test message"
```
Check the `relevant log` file or `journal` to verify the message.

## 9. Troubleshooting and Security
Useful commands :
```bashsystemctl status rsyslog
journalctl -u rsyslog
ss -tulnp
ip a
```

### Common troubleshooting checks :
- Is the logging service running?

- Is the configuration syntax valid?

- Is the correct UDP or TCP port listening?

- Is the firewall allowing traffic?

- Can the client reach the server?

- Are log files writable by the logging service?

### Security considerations :
- Protect log files from unauthorized access and modification.

- Use `TLS` for remote logging over untrusted networks.

- Restrict access to the logging server.

- Monitor disk usage to prevent logs from filling the filesystem.

- Use log rotation to manage old log files.

- Do not blindly trust remote log messages; validate their source when investigating incidents.
