# Linux Process Management
## ps
Display information about running processes.
```bash
ps
ps -a
ps -f
ps -l
ps -aux
```
Search for a specific process:
```bash
ps -aux | grep gparted
ps -aux | grep gedit
```
### User Processes
```bash
ps -u hesam
ps -u hesam --forest
```
### Process Tree
```bash
ps -He
ps -axjf
```

## kill
Terminate a process using its PID.
```bash
kill PID
```
Example:
```bash
kill 1234
```
> Prefer graceful termination before using stronger signals.
## top
Real-time process and system monitoring:
```bash
top
```
Useful interactive commands inside `top`:
```
k → send signal to a process
r → change process priority
p → sort by CPU usage
m → sort/display memory information
s → change refresh interval
u → filter by user
q → quit
```

## Load Average
Linux load average is commonly displayed for:
```
1 minute
5 minutes
15 minutes
```
Load average represents the amount of work competing for CPU and certain system resources. It should be interpreted together with CPU count and system activity.

## Memory
```bash
free
```
Detailed output:
```bash
free -h
```

## htop
Interactive process viewer:
```bash
htop
```
