# Linux Processes

*Process* is a running instance of a program.

## Basic commands

- `ps aux` - show info about processes. Params:
  * `-a` - all users
  * `-u` - additional info like `-f` option
  * `-x` - info about processes without terminals
  * `-f` - full info with process tree
  * `-e` - extended info with all args for each process
- `ps aux | head -1; ps aux | sort -rnk 4 | head -5` - show top 5 processes with highest memory usage
- `pstree` - show tree of processes
- `kill -9 500` - kill process with PID = 500
- `sleep 1000 &` - run process in the background
- `jobs` - show processes running in the context of the current shell
- `watch date` - run a command repeatedly (2 seconds cycle by default)
- `ss -4tunlp | grep 514` - check if some process is listening port 514
- `netstat -tunlp` - show used ports

### Run a command in the background

```bash
php artisan horizon > /dev/null 2>&1 &
```

Redirect the `stdout` and `stderr` to `/dev/null` to ignore the output.

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
