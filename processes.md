# Linux Processes

*Process* is a running instance of a program.

## Commands

- `ps aux` - show info about processes. Params:
  * `-a` - all users
  * `-u` - additional info like `-f` option
  * `-x` - info about processes without terminals
  * `-f` - full info with process tree
  * `-e` - extended info with all args for each process
- `ps aux | head -1; ps aux | sort -rnk 4 | head -5` - show top 5 processes with highest memory usage
- `pstree` - show tree of processes
- `pstree | head -n 5` - show current system manager ("systemd" - you are using systemd, "init" - SysVinit)
- `systemctl list-units --type=service` - list services on Linux on a systemd system. Specify the "--type=service" option in order to restrict the results to services only
- `systemctl list-unit-files --type=service` - list all service files available
- `top -U sales` - show CPU, memory for processes of user sales
- `sudo pmap 155` - how much memory the process with PID 155 is using
- `kill -9 500` - kill process with PID = 500
- `pgrep -p some_worker` - show PID of the process named 'some_worker'
- `pkill -p some_worker` - kill process named 'some_worker'. For debug use `pgrep` to get the PID of the process that will be terminated.
- `sleep 1000 &` - run process in the background
- `jobs` - show processes running in the context of the current shell
- `watch date` - run a command repeatedly (2 seconds cycle by default)

### Ports

- `ss -4tunlp | grep 514` - check if some process is listening port 514
- `netstat -tunlp`, `netstat -peanut` - show used ports
- `lsof` - LiSts all Open Files belonging to all active processes
- `lsof -i :8000` - show the application used by that port with PID

### Command `top`

- `e` - switch units - kB, Mb, Gb...
- `Shift + e` - switch units in the header
- `1` - show CPU load info
- `t` - swap the CPU displays to simple ASCII graphs that show the percentage of usage for each CPU
- `m` - display memory, swap in diffetent formats
- `y` - highlight running tasks in the process list
- `x` - highlight the column used to sort the process list.
- `u` - see the processes for a single user
- `i` - see only active tasks. Tasks that haven't consumed any CPU since the last update won't be shown

*top* provides some fields:

- `VIRT` -- Virtual Image (kb): The total amount of virtual memory used by the task. *Virtual* is maximum amount of memory the process could theoretically use if it were the only process (never so), used every single page it allocated (never happens) and didn't map or unmap any pages (unlikely).
- `RES` -- Resident size (kb): The non-swapped physical memory a task has used ; RES = CODE + DATA.
- `DATA` -- Data+Stack size (kb): The amount of physical memory devoted to other than executable code, also known as the 'data resident set' size or DRS.
- `SHR` -- Shared Mem size (kb): The amount of shared memory used by a task. It simply reflects memory that could be potentially shared with other processes.

### Run a command in the background

```bash
php artisan horizon > /dev/null 2>&1 & disown
```

Redirect the `stdout` and `stderr` to `/dev/null` to ignore the output. `disown` helps to prevent killing the process when console is closed.

## Create process

A new process is normally created when an existing process makes an exact copy of itself in memory. 
The child process will have the same environment as its parent, but different PID.

## Debug processes

### strace

- `strace ls` - trace command
- `strace -e trace=open -f ping -c1 google.com` - trace `ping -c1 google.com` command
- `strace -p 1000` - trace process by PID
- `strace -c -p 1000` - show all system calls for PID
- `strace -t df -h` - show time of each system call
- `strace -e trace=open,read -p 1000 -o debug.txt` -  log a trace of the open, read system calls to file

## Process Priority and Nice Value

In Linux priorities are 0 to 139 in which 100 to 139 for users. So command `top` shows `PR` (Priority) from 0 to 39.

- Priority: `0 ... 39`. Priority `0` is the highest.
- Nice: `-20 ... 19`. Process with nice `-20` has highest priority - it struggles for the CPU, so it isn't "nice guy".

```
Priority = Nice + 20
```

### Start the process with the nice value

```bash
nice -n 15 bash loop.sh &
```

Process will get nice to `15`, priority will be `35` (as priority = 20 + nice). 
As it has the least priority, it gets the least amount of CPU.

### Change nice of the process

```bash
renice -n 5 -p 123
```
Nice value of process PID 123 will be set to `5`, priority will be 25. The CPU will be allocated accordingly.
