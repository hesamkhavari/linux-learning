# Basic Linux Security
## User Identity
```bash
id
```
Display information about a specific user:
```bash
id hesam
```
Current username:
```bash
whoami
```
## Switching Users
```bash
su hesam
```
Return to the previous shell:
```bash
exit
```
Root shell:
```bash
su
```
> Prefer controlled privilege escalation and avoid unnecessary root sessions.
## User Database
User account information is stored in:
```bash
/etc/passwd
```
Group information:
```bash
/etc/group
```
Password-related information is stored in:
```bash
/etc/shadow
```
Query a user through NSS:
```bash
getent passwd hesam
```
Search for a user:
```bash
grep '^hesam:' /etc/passwd
```
## Service / System Accounts
Example:
```bash
grep '/nologin' /etc/passwd
```
This can help identify accounts that normally cannot be used for interactive login.
## Logged-in Users
```bash
who
```
Detailed information:
```bash
w
```
Boot time:
```bash
who -b
```
Runlevel / system state:
```bash
who -r
```
All available information:
```bash
who -a
```
Number of logged-in users:
```bash
who -q
```
Login history:
```bash
last
```
## Important Security Notes
Files such as `/etc/shadow` contain sensitive authentication data.
Do not modify these files directly unless you understand the consequences.
Use dedicated tools such as:
```bash
passwd
usermod
useradd
```
for normal account administration.
