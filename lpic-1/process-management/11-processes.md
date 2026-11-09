# Process Creation, Monitoring and Termination

## 1. Creating and Interrupting Processes
```bash
sleep 444
```
Runs `sleep` in the foreground and waits for 444 seconds.

- `Ctrl+C` : sends SIGINT to the foreground process group in a typical interactive shell.

- `Ctrl+Z` : suspends the foreground job in a typical interactive shell.

## 2. Running Processes in the Background
```bash
sleep 555 &
sleep 666 &
jobs
```
The `&` operator starts a command asynchronously in the background.

`jobs` displays jobs managed by the current shell.

Example output may look like :
```Plain text
[1]- Running    sleep 555 &
[2]+ Running    sleep 666 &
```
Job numbers are assigned by the shell and can change between sessions.

## 3. Foreground and Background Jobs
```bash
fg %2
```
Brings job 2 to the foreground.

Press `Ctrl+Z` to suspend it, then run :
```bash
bg %2
jobs
```
- `fg %job` : resume a job in the foreground.

- `bg %job` : resume a suspended job in the background.

- `jobs` : list jobs belonging to the current shell.

Use the actual job number shown by jobs; %2 is only an example.

## 4. Switching Users and Shell Job Control
```bash
su
sleep 897 &
jobs
```
`su` starts a shell as another user, subject to authentication and system policy. 
The new shell has its own job table, so jobs there does not display the parent shell's jobs.

To leave the su shell:
```bash
exit
```
Use `sudo` where appropriate if you only need to run one command with elevated privileges.

## 5. Listing and Finding Processes
```bash
ps aux
ps aux | grep sleep
```
- `ps aux` : displays a snapshot of processes, including processes belonging to other users when permissions allow.

- `grep sleep` : filters displayed lines containing sleep; the output may also include the grep command itself.

A more targeted alternative is:
```bash
pgrep -a sleep
```
Common process identification tools:

Command|Purpose
|---|---|
ps|Display a process snapshot
pgrep|Find process IDs by name or other criteria
pidof|Find PIDs for a program, where supported
jobs|List current-shell jobs
top|Monitor processes interactively

## 6. Terminating Processes with Signals
```bash
kill PID
kill -TERM PID
kill -KILL PID
```
Replace `PID` with the actual numeric process ID. Do not type the placeholder literally.

- `kill PID` sends SIGTERM by default.

- `SIGTERM (15)` requests graceful termination.

- `SIGKILL (9)` forces termination and cannot be caught or handled by the target process.

Prefer SIGTERM first :
```bash
kill -TERM 12345
```
Use SIGKILL only when necessary :
```bash
kill -KILL 12345
```
The example PID 12345 is illustrative; verify the actual PID before running a command.

### Using killall
```bash
killall sleep
```
Sends a signal to processes matching the specified name, subject to permissions and the system's implementation.

Be careful: it may affect multiple processes. For a controlled exercise, identify and terminate only the process you created.

## 7. Keeping a Command Running After Logout
```bash
nohup sleep 123 &
```
`nohup` makes a command ignore the hangup signal (SIGHUP). The `&` starts it in the background.

By default, output may be redirected to `nohup.out` when appropriate.

`nohup` does not provide full service supervision, automatic restart, or a guarantee that the process will survive every logout or system shutdown.

## 8. Monitoring Processes with top
```bash
top
top -u hesam
```

- `top` : interactively displays processes and system resource usage.

- `top -u hesam` : filters processes by the specified user on common procps implementations.

### Common interactive keys:

Key|Purpose
|---|---|
h|Display help
q|Quit
P (Shift+P)|Sort by CPU usage on common implementations
M (Shift+M)|Sort by memory usage
k|Prompt to send a signal to a process
c|Toggle full command-line display
d|Change refresh delay
1|Toggle per-CPU display on common implementations
z|Toggle color display on common implementations
< / >|Change the sort field on supported versions

Key behavior can vary slightly by top implementation and version. Press `h` inside top to check the available commands.

When using `k`, verify the target PID and signal before confirming.

## 9. Monitoring System Uptime and Memory
```bash
uptime
free
free -h
```

- `uptime` : shows current time, how long the system has been running, logged-in users, and load averages.

- `free` : reports memory and swap usage.

- `free -h` : displays memory values in human-readable units.

## 10. Safe Practice Exercise
Use your own test process:
```bash
sleep 600 &
jobs
pgrep -a sleep
ps -o pid,ppid,stat,cmd -p PID
kill -TERM PID
```

Replace PID with the process ID identified in the previous step. Then verify whether it has exited:
```bash
pgrep -a sleep
```

If several sleep processes exist, do not terminate all of them just to clean up the exercise.

Key distinction: shell jobs are managed by the current shell; 
processes are managed by the operating system. 
A background job is still a process, but not every process is a job in your current shell.
