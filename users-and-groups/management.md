# Linux Users and Groups Management
### Linux uses local account databases such as `/etc/passwd`, `/etc/shadow`, and `/etc/group` for local user and group management.

## User Information
View local users:
```bash
cat /etc/passwd
tail /etc/passwd
```
User entries in /etc/passwd contain fields such as:
```Plaintext
username:x:UID:GID:comment:home:shell
```
Password hashes are stored separately in:
```bash
sudo cat /etc/shadow
```
> `/etc/shadow` contains sensitive authentication information and should not normally be edited directly.

## useradd
Create a user:
```bash
sudo useradd user1
```
Create a user with a specific UID:
```bash
sudo useradd -u 1330 user2
```
Specify a home directory:
```bash
sudo useradd -u 1111 -d /home/user3 user3
```
Create a user with a non-login shell:
```bash
sudo useradd -u 1555 -d /home/user4 -s /usr/sbin/nologin user4
```
> The exact path of nologin can vary. Check it with:
```bash
command -v nologin
```

## Create a Home Directory
On systems where home creation is not enabled by default:
```bash
sudo useradd -m user1
```
### The `-m` option creates the home directory and copies files from the skeleton directory.

## /etc/skel
Default files for newly created user home directories are commonly stored in:
```bash
cd /etc/skel/
ls -la
```
### When a home directory is created, files from `/etc/skel` can be copied into it.

## useradd Defaults
Display current default values:
```bash
useradd -D
```
Configuration can also be affected by:
```bash
/etc/default/useradd
/etc/login.defs
```
The exact defaults depend on the distribution and `shadow-utils` configuration.

## passwd
Set or change a user's password:
```bash
sudo passwd user1
```
Check password status:
```bash
sudo passwd -S user1
```
Lock an account password:
```bash
sudo passwd -l user1
```
Unlock it:
```bash
sudo passwd -u user1
```
> Password locking and account expiration are related but different concepts.

## Passwords and useradd
Avoid putting real passwords directly on the command line:
```bash
useradd -p 'password' user1
```
### Command-line passwords can be exposed through process or shell history mechanisms.
Prefer:
```bash
sudo useradd user1
sudo passwd user1
```

## usermod
Modify an existing user:
```bash
sudo usermod -c "User Full Name" user1
```
Change UID:
```bash
sudo usermod -u 9876 user1
```
Change login shell:
```bash
sudo usermod -s /usr/sbin/nologin user1
```
Add a supplementary group:
```bash
sudo usermod -aG group1 user1
```
> `-aG` is important. Using `-G` without `-a` replaces the user's existing supplementary groups.

## chage
Manage password aging and expiration:
```bash
sudo chage -l user1
```
Set account expiration:
```bash
sudo chage -E 2027-09-01 user1
```
Interactive configuration:
```bash
sudo chage user1
```
### `chage` manages password expiration information such as minimum/maximum age, warning period, and account expiration.

## userdel
Delete a user:
```bash
sudo userdel user1
```
Delete the user and its home directory:
```bash
sudo userdel -r user1
```
> `-r` should be used carefully because user-owned data may be deleted.

## Finding Files by UID
Find files owned by a specific UID:
```bash
sudo find / -uid 1330 2>/dev/null
```

## Groups
### Create a Group
```bash
sudo groupadd group1
```
Create a group with a specific GID:
```bash
sudo groupadd -g 2700 group2
```
View groups:
```bash
cat /etc/group
tail /etc/group
```

## Add a User to Supplementary Groups
```bash
sudo usermod -aG learn user1
sudo usermod -aG teach user1
```
Verify:
```bash
id user1
groups user1
```

## Modify a Group
Change GID:
```bash
sudo groupmod -g 3000 learn
```
Rename a group:
```bash
sudo groupmod -n learning learn
```

## Delete a Group
```bash
sudo groupdel group2
```

## Important Files
|File|	Purpose|
|:---|:---:|
|`/etc/passwd`	|Local user account information|
|`/etc/shadow`	|Password hashes and password aging|
|`/etc/group`	|Local group information|
|`/etc/skel/`	|Default files for new home directories|
|`/etc/login.defs`	|Login/account defaults|
|`/etc/default/useradd`	|`useradd` defaults on systems using it|

## Practical Verification
After modifying an account:
```bash
id user1
getent passwd user1
getent group group1
```
Using `getent` is preferable when you want to query the system's configured Name Service Switch rather than only reading local files.
