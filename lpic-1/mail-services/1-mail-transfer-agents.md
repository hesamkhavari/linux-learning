# Mail Transfer Agents (MTA)

## 1. What Is an MTA?

A **Mail Transfer Agent (MTA)** is software that sends and receives email messages between mail servers.

An MTA can:
- Accept email messages.
- Transfer messages to other mail servers.
- Store messages in a mail queue.
- Deliver messages to local mailboxes.

### Common Email Components

| Component | Purpose |
|---|---|
| MUA | Mail User Agent: reads and writes emails |
| MTA | Mail Transfer Agent: transfers emails |
| MDA | Mail Delivery Agent: delivers emails to mailboxes |

## 2. Common MTA Programs

| MTA | Description |
|---|---|
| Sendmail | Traditional MTA with a powerful but complex configuration |
| Qmail | Security-focused historical MTA with its own architecture |
| Exim | Flexible MTA with advanced ACL and authentication features |
| Postfix | Modular MTA with relatively simple configuration; widely used today |

**Important:** Postfix and Exim are common modern choices. Sendmail and Qmail are also important to know for compatibility and legacy systems.

## 3. Sendmail Compatibility Interface

Different MTAs have their own configuration files, but many support commands and interfaces compatible with Sendmail.

This is commonly called **Sendmail compatibility** or a Sendmail-compatible interface.

Examples:

```bash
sendmail    # Send a message using a compatible interface
mailq       # Display the mail queue
newaliases  # Rebuild the aliases database
```

These commands may be provided by the installed MTA or compatibility packages. Their availability depends on the system.

## 4. Important Configuration Files

| File | Purpose |
|---|---|
| `/etc/aliases` | System-wide email aliases |
| `/home/USER/.forward` | Forward email for one user |
| `/etc/postfix/main.cf` | Main Postfix configuration |
| `/etc/postfix/master.cf` | Postfix service configuration |

Each MTA has its own configuration format and files.

## 5. Email Aliases

Aliases redirect email addressed to one local name to another user or address.

The system-wide aliases file is:

```bash
sudo nano /etc/aliases
```

Example:

```text
postmaster: root
hostmaster: root
support: hesam
```

Multiple destinations are also possible:

```text
admin: hesam, root
```

After editing the file, rebuild the aliases database:

```bash
sudo newaliases
```

Check an alias:

```bash
getent aliases support
```

**Important:** `/etc/aliases` is system-wide. The `newaliases` command is commonly supported by Sendmail-compatible MTAs.

## 6. Email Forwarding for a User

A normal user can forward incoming email using a `.forward` file in their home directory.

Example:

```bash
su - hesam
nano ~/.forward
```

Add a destination:

```text
root
```

Or use an email address:

```text
example@example.com
```

Save the file.

This forwarding applies only to that user's incoming local mail.

**Note:** `.forward` support depends on the MTA and its local delivery configuration. The file should be owned by the user and must not be writable by other users.

## 7. The `mail` Command

The `mail` command is a simple command-line mail client.

Send a message:

```bash
mail -s "Test Email" hesam
```

Type the message body and press `Ctrl+D` to finish.

Send a message using a pipe:

```bash
echo "Hello Hesam" | mail -s "Test" hesam
```

Send a multi-line message:

```bash
mail -s "System Report" hesam <<EOF
Hello,
This is a test message.
Regards,
Root
EOF
```

Read received messages:

```bash
mail
```

Inside the mail interface, common commands include:

| Command | Purpose |
|---|---|
| `1` | Open message number 1 |
| `d` | Delete the current message |
| `q` | Quit and save changes |
| `h` | Display message headers |

The exact commands may vary between mail clients.

## 8. Installing and Testing Postfix

Postfix is a common MTA for Linux systems.

### Debian / Ubuntu

Install Postfix and a mail client:

```bash
sudo apt update
sudo apt install postfix mailutils
```

During installation, select **Local only** for a local testing environment.

Check the service:

```bash
sudo systemctl status postfix
```

Start and enable it:

```bash
sudo systemctl enable --now postfix
```

Check the version:

```bash
postconf mail_version
```

### Basic Postfix Configuration

Main configuration file:

```text
/etc/postfix/main.cf
```

View important settings:

```bash
postconf -n
```

For a local-only test environment, make sure these settings are present:

```text
inet_interfaces = loopback-only
mydestination = $myhostname, localhost.$mydomain, localhost
```

This limits SMTP listening to the local machine.

After changing the configuration:

```bash
sudo postfix check
sudo systemctl reload postfix
```

If necessary:

```bash
sudo systemctl restart postfix
```

**Warning:** Do not expose an unconfigured mail server to the public Internet. An incorrectly configured server can become an open relay.

## 9. Testing Local Email Delivery

Create a local test user:

```bash
sudo adduser mailtest
```

Send a test email:

```bash
echo "Hello from Linux" | mail -s "Local Mail Test" mailtest
```

Read the email as that user:

```bash
sudo -iu mailtest
mail
```

Useful commands:

```bash
mailq                  # Show queued messages
postqueue -p           # Show Postfix queue
postfix status         # Check Postfix status
postfix check          # Check configuration
```

## 10. Mail Queue Management

The mail queue stores messages waiting for delivery.

Useful Postfix commands:

```bash
postqueue -p           # Display queue
postqueue -f           # Request immediate queue processing
postsuper -d QUEUE_ID  # Delete one queued message
```

Delete all queued messages:

```bash
sudo postsuper -d ALL
```

**Warning:** Deleting the queue is destructive. Check the queue before removing messages.

## 11. Logs and Troubleshooting

Mail logs help identify delivery and configuration problems.

Common log locations:

```text
/var/log/mail.log       # Common on Debian / Ubuntu
/var/log/maillog        # Common on RHEL-based systems
```

Useful commands:

```bash
sudo tail -f /var/log/mail.log
sudo journalctl -u postfix
sudo systemctl status postfix
sudo postfix check
sudo postqueue -p
```

Common problems:

- Postfix service is not running.
- Incorrect aliases configuration.
- Missing or incorrect local user.
- DNS or network problems.
- Incorrect hostname or domain configuration.
- Messages remain in the mail queue.
- Another service is already using SMTP port 25.

## Important Notes

- MTA transfers email between systems.
- MUA is used to read and write email.
- MDA delivers messages to mailboxes.
- `/etc/aliases` contains system-wide aliases.
- `newaliases` rebuilds the aliases database.
- `~/.forward` configures forwarding for an individual user when supported.
- `mail` is a simple command-line mail client.
- `mailq` and `postqueue -p` display queued messages.
- Postfix uses `/etc/postfix/main.cf` and `/etc/postfix/master.cf`.
- SMTP commonly uses TCP port 25 for server-to-server mail transfer.
- Successful Internet email delivery requires more than installing an MTA; DNS, authentication, and security configuration also matter.
