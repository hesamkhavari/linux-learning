# Task Scheduling in Linux

Linux provides tools to run tasks automatically at specific times.

### The main tools are :
- `cron` : Runs tasks repeatedly at scheduled times.

- `at` : Runs a task once at a specific time.

- `anacron` : Runs periodic tasks even if the system was offline at the scheduled time.

## 1. Cron
`Cron` is used to schedule recurring tasks, such as daily backups or weekly maintenance scripts.
### Cron Configuration Locations

Location|Purpose
|---|---|
`/etc/crontab`|System-wide cron jobs
`/etc/cron.d/`|Additional system-wide cron files
`/etc/cron.hourly/`|Hourly scripts
`/etc/cron.daily/`|Daily scripts
`/etc/cron.weekly/`|Weekly scripts
`/etc/cron.monthly/`|Monthly scripts
`User crontabs`|Jobs defined by individual users

> Scripts in periodic directories are usually executed by `run-parts` and may be controlled by `anacron`. Their exact execution time is not always guaranteed.

### Cron Syntax
A `user crontab` contains five time fields followed by a command :
```bash
* * * * * command
| | | | |
| | | | +--- Day of week (0-7)
| | | +----- Month (1-12)
| | +------- Day of month (1-31)
| +--------- Hour (0-23)
+----------- Minute (0-59)
```
Common examples :
```bash
* * * * * command       # Every minute
*/5 * * * * command     # Every 5 minutes
0 * * * * command       # Every hour
0 2 * * * command       # Every day at 02:00
0 3 * * 0 command       # Every Sunday at 03:00
0 4 1 * * command       # First day of each month
```
Special strings :
```bash
@reboot command
@daily command
@weekly command
@monthly command
@yearly command
```
### Managing User Crontabs
```bash
crontab -e       # Edit current user's crontab
crontab -l       # List current user's jobs
crontab -r       # Remove current user's crontab
crontab -u user -l  # List another user's crontab (requires permission)
```
A `user crontab` does not need a username field. Jobs run with the permissions of the `user who owns` that crontab.

### System-Wide Cron Files
The format of `/etc/crontab` and files inside `/etc/cron.d/` includes an additional username field :
```bash
MIN HOUR DAY MONTH WEEKDAY USER COMMAND
```
Example :
```bash
5 * * * * root /usr/local/bin/maintenance.sh
```
The command runs as root at minute 5 of every hour.

### Important Cron Notes
- Use absolute paths for commands and scripts.

- `Cron` has a limited environment compared to an interactive shell.

- Make sure scripts have the correct permissions.

- Use `MAILTO` in a `crontab` to define an email recipient for job output.

- Email delivery requires a configured mail system.

- Check the cron service :
```bash
systemctl status cron     # Debian / Ubuntu
systemctl status crond    # RHEL-based systems
```

## 2. Anacron
`Anacron` is useful for systems that are not always running.

For example, if a daily task was missed because a laptop was `powered off`, anacron can run it when the system `becomes available` again.

Common configuration :
```bash
/etc/anacrontab
```
Example :
```bash
1  5  daily-job  /usr/local/bin/backup.sh
```
The fields define the delay in days, delay in minutes, job identifier, and command.

> Some systems use files such as `/etc/cron.hourly/0anacron` to trigger anacron checks.

## 3. At
`at` schedules a command to run once at a future time.

Examples :
```bash
at 22:00
at now + 1 hour
at now + 1 week
at teatime
```
After starting at, enter the commands and press `Ctrl+D` to submit the job.

Example :
```bash
at 22:00
/usr/bin/touch /home/user/test.txt
Ctrl+D
```

### Managing `At` Jobs
```bash
atq          # List pending jobs
atrm 1       # Remove job number 1
```
The `at` service is commonly managed by :
```bash
systemctl status atd
```

## 4. Cron and At Access Control
Linux can restrict which users are allowed to schedule jobs.

File|Purpose
|---|---|
`/etc/cron.allow`|Users allowed to use `cron`
`/etc/cron.deny`|Users denied access to `cron`
`/etc/at.allow`|Users allowed to use `at`
`/etc/at.deny`|Users denied access to `at`

### Important rules :

- If `cron.allow` exists, only users listed in it can use cron. `cron.deny` is ignored.

- If only `cron.deny` exists, listed users are denied.

- Similar rules apply to `at.allow` and `at.deny`.

- If neither file exists, default behavior depends on the distribution and configuration.

Example :
```bash
sudo nano /etc/cron.deny
```
Add a username to deny access :
```bash
hesam
```
Check the access-control files :
```bash
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/at.allow
cat /etc/at.deny
```

## 5. Troubleshooting and Security
When investigating scheduled tasks on a Linux server, inspect all relevant locations :
```bash
crontab -l
sudo crontab -l
cat /etc/crontab
ls -la /etc/cron.d/
ls -la /etc/cron.hourly/
ls -la /etc/cron.daily/
ls -la /etc/cron.weekly/
ls -la /etc/cron.monthly/
atq
```
Check cron logs :
```bash
journalctl -u cron
journalctl -u crond
```
On some distributions, cron activity is also recorded in :
```bash
/var/log/syslog
/var/log/cron
```
### Security considerations :

- Review scheduled jobs when auditing an unfamiliar server.

- Check script ownership, permissions, and contents.

- Pay special attention to jobs running as root.

- Do not blindly delete unfamiliar jobs. Identify their purpose and owner before changing them.

- Avoid editing `cron spool` files directly. Use `crontab -e`.

- The location of `user crontabs` varies by distribution.
- For example, Debian-based systems commonly use `/var/spool/cron/crontabs/`, while RHEL-based systems commonly use `/var/spool/cron/`.

### Key Differences

Feature|cron|at|anacron
|---|---|---|---|
Execution|Repeated|One-time|Periodic
Scheduling|Minute/hour/day|Specific future time|Day-based intervals
Missed jobs|Usually not recovered|Runs at scheduled time if system is available|Can run missed periodic jobs later
Common use|Recurring maintenance|One-time task|Periodic tasks on machines not always running
