# User and Group Management

## 1. User Accounts
Linux uses user accounts to control access to files, services, and system resources.
### There are two common types of users :
- **Regular Users** : Used by people to log in and work.

- **System Users** : Used by services and system processes.

System accounts often use a non-login shell such as `/usr/sbin/nologin`.

## 2. Important Account Files

File|Purpose
|---|---|
`/etc/passwd`|User account information
`/etc/shadow`|Password hashes and password aging
`/etc/group`|Group information
`/etc/gshadow`|Secure group information

### /etc/passwd
Stores `basic` user information.
```bash
username:x:UID:GID:comment:home_directory:shell
```
### /etc/shadow
Stores password `hashes` and password aging information.
It is normally readable only by `root` or authorized system processes.

### /etc/group
Stores group information.
```bash
group_name:x:GID:user1,user2
```

### /etc/gshadow
Stores secure group information, including group administrators and protected group passwords.

> **Important** : Do not edit these files manually unless you know exactly what you are doing. Use account management commands instead.

## 3. Create Users
Create a user with a home directory :
```bash
sudo useradd -m user1
```
Create a user with custom settings :
```bash
sudo useradd -m \
  -d /home/user1 \
  -c "Hesam Khavari" \
  -s /bin/bash user1
```
Set the user's password :
```bash
sudo passwd user1
```

### Common useradd Options

Option|Purpose
|---|---|
`-m`|Create a home directory
`-d DIR`|Set the home directory
`-s SHELL`|Set the login shell
`-c TEXT`|Add a comment
`-u UID`|Set the user ID
`-g GROUP`|Set the primary group
`-G GROUPS`|Set supplementary groups

## 4. Modify Users
Change the login shell :
```bash
sudo usermod -s /bin/sh user1
```
Change the comment :
```bash
sudo usermod -c "IT Engineer" user1
```
Change the home directory :
```bash
sudo usermod -d /home/newuser -m user1
```
Add a user to supplementary groups :
```bash
sudo usermod -aG adm,mysql user1
```

> **Important** : Use `-aG` to add groups without removing existing supplementary group memberships.

Check user information :
```bash
id user1
groups user1
getent passwd user1
```

## 5. Delete Users
Delete a user but keep the home directory :
```bash
sudo userdel user1
```
Delete a user and their home directory :
```bash
sudo userdel -r user1
```

> **Warning** : Check the user's files and running processes before deleting an account.

## 6. Password Management
Set or change a user's password :
```bash
sudo passwd user1
```
Change your own password :
```bash
passwd
```
Lock a user's password :
```bash
sudo usermod -L user1
```
Unlock a user's password :
```bash
sudo usermod -U user1
```
Check password status :
```bash
sudo passwd -S user1
```

> **Important** : Locking a password does not always disable every login method. `SSH` keys and other authentication methods may still work.

## 7. Switch Users
Switch to another user :
```bash
su user1
```
Start a login shell for another user :
```bash
su - user1
```
Return to the previous shell :
```bash
exit
```

> **Note** : `su -` loads the target user's login environment.

## 8. Group Management

Groups allow multiple users to share permissions.

Create a group :
```bash
sudo groupadd friends
```
Create a group with a specific GID :
```bash
sudo groupadd -g 666999 friends
```
Add a user to a group :
```bash
sudo usermod -aG friends user1
```
Change a group's GID :
```bash
sudo groupmod -g 777888 friends
```
Rename a group :
```bash
sudo groupmod -n newname friends
```
Delete a group :
```bash
sudo groupdel friends
```
Check group information :
```bash
getent group friends
```

### Important :
- GID must be unique.

- A group cannot normally be deleted while it is the primary group of a user.

- Check existing group memberships before changing or deleting groups.

## 9. Password Aging with chage
The `chage` command manages password expiration and account aging.
Show password aging information :
```bash
sudo chage -l user1
```
Set account expiration date :
```bash
sudo chage -E 2028-01-01 user1
```
Set maximum password age :
```bash
sudo chage -M 90 user1
```
Set minimum password age :
```bash
sudo chage -m 7 user1
```
Set a warning period before expiration :
```bash
sudo chage -W 7 user1
```
Force a password change at the next login :
```bash
sudo chage -d 0 user1
```

### Common chage Options
Option|Purpose
|---|---|
`-l`|List account aging information
`-E`|Set account expiration date
`-M`|Set maximum password age
`-m`|Set minimum password age
`-W`|Set password expiration warning days
`-d`|Set last password change date

## 10. System Accounts

System accounts are commonly used by services and applications.

Example :
```bash
sudo useradd -r -s /usr/sbin/nologin appuser
```
- `-r` : Create a system account.

- `-s` : Set the login shell.

- `nologin` : Prevent normal interactive login.

Check the account :
```bash
getent passwd appuser
```
The exact `nologin` path may differ between distributions.

## 11. Useful Commands

Command|Purpose
|---|---|
`id USER`|Show UID, GID, and groups
`whoami`|Show current username
`who`|Show logged-in users
`getent passwd USER`|Show user account information
`getent group GROUP`|Show group information
`groups USER`|Show group memberships
`last`|Show login history
`passwd -S USER`|Show password status
`chage -l USER`|Show password aging information

## 12. Important Notes
- UID identifies a user.

- GID identifies a group.

- Every user has a primary group.

- A user can belong to multiple supplementary groups.

- `/etc/passwd` contains basic user information, not password hashes.

- `/etc/shadow` stores password hashes and aging information.

- Use `usermod -aG` when adding supplementary groups.

- Use `userdel -r` carefully because it removes the user's home directory.

- Use `nologin` for service accounts that should not have normal interactive access.

- Use `getent` to query account information through the system's configured user databases.
