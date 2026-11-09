# Process Priority and Nice Values

## 1. Understanding Process Priority
### Linux uses scheduling policies to determine how processes and threads receive CPU time. 
For ordinary processes using the standard time-sharing scheduling policy, the `nice` value influences how favorably they are scheduled.

The usual Linux nice-value range is -20 to 19 :

Nice value|Relative scheduling preference
|---|---|
-20|Highest
-10|Higher than default
0|Default
10|Lower than default
19|Lowest

A lower `nice` value generally gives a process a greater share of CPU time relative to competing processes under the same scheduling policy. 
**It does not guarantee that the process will run faster.**

## 2. Viewing Process Information
```bash
ps
ps aux
ps -alx
ps -eo pid,pcpu,nice,comm,user
ps -eo user,pid,comm,nice
pstree
top
```

- `ps` : displays a snapshot of processes.

- `ps aux` : displays detailed process information.

- `ps -alx`: displays processes in a detailed BSD-style format.

- `ps -eo` : selects the output columns explicitly.

- `pstree` : displays processes in a parent-child tree.

- `top` : interactively monitors processes and resource usage.

Example :
```bash
ps -eo pid,pcpu,nice,comm,user
```
Displays process ID, CPU usage, nice value, command name, and user.

To find processes by name :
```bash
ps -eo pid,pcpu,nice,comm,user | grep '[s]leep'
```
The [s]leep pattern helps avoid matching the grep command itself.

## 3. Starting a Process with nice
```bash
sleep 6000 &
nice -n 15 sleep 6001 &
```
The first command starts a background process with the shell's inherited nice value, usually `0`. The second requests a nice value of `15`.

Verify the values :
```bash
ps -eo pid,ni,comm | grep '[s]leep'
```
The `NI` or `ni` column represents the `nice` value.

Important: `nice -n N` specifies an adjustment to the current nice value in GNU nice, not necessarily an absolute target value. 
For an ordinary user starting from nice 0, `nice -n 15` normally results in nice 15.

## 4. Requesting Higher CPU Scheduling Preference
```bash
sudo nice -n -10 sleep 6002 &
```
A negative nice value requests more favorable scheduling. 
Setting a lower nice value normally requires elevated privileges or an applicable resource limit.

Do not use elevated privileges unnecessarily. For learning, compare ordinary nice values first.

## 5. Changing an Existing Process with renice
```bash
renice -n 14 -p PID
```
Replace PID with the actual numeric process ID.

This sets the target process's nice value to 14 on common Linux `renice` implementations. 
An ordinary user can generally make their own processes less favorable by increasing the nice value, 
but cannot arbitrarily increase their priority by lowering it.

Verify the result :
```bash
ps -o pid,ni,comm -p PID
```
To adjust processes belonging to a user :
```bash
sudo renice -n 17 -u USERNAME
```

Replace `USERNAME` with the intended account. 
This affects matching processes owned by that user, subject to system and command implementation behavior. 
Review the target carefully before changing multiple processes.

## 6. Observing Nice Values with top
```bash
top
```
The `NI` column shows the nice value. 
The `PR` column shows the scheduler priority as reported by top; it is not the same field as NI.

Inside `top`, press `h` for help. The available commands and key behavior can vary by version.

## 7. Nice Values and Other System Activity
```bash
ping 127.0.0.1
ps -eo pid,ni,user,comm
```

To observe CPU scheduling effects, nice values matter most when multiple CPU-bound processes compete for CPU time under comparable scheduling policies.

## 8. Important Distinctions

- `nice` starts a command with an adjusted nice value.

- `renice` changes the nice value of an existing process or selected group of processes.

- `NI` represents the nice value.

- `PR` represents a scheduler priority display and should not be confused with the nice value.

- Nice values influence CPU scheduling; they do not directly set memory limits or guarantee a fixed percentage of CPU time.

- Real-time scheduling policies use different priority mechanisms and are not governed in the same way by ordinary nice values.
