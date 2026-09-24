# Processes and Jobs
Linux provides several mechanisms for controlling foreground and background jobs from the shell.
## Background Execution
A command can be started in the background using `&`.
```bash
gparted &
```
## Jobs
Displays jobs associated with the current shell.
```bash
jobs
```
## bg
Resumes a suspended job in the background.
```bash
bg
```
## fg
Brings a background job to the foreground.
```bash
fg
```
## Jobs Control Shortcuts
- `Ctrl+Z`
Suspends the current foreground job.
- `Ctrl+C`
Interrupts the current foreground command.
## su
su starts a shell or command under another user account, subject to authentication and system permissions.
```bash
su
```
## exit
Exits the current shell or session.
```bash
exit
```

### Important Concept
Foreground and background execution are fundamental concepts for working with Linux processes from the shell.
