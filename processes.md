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
- `strace -p 1000` - trace process by PID
- `strace -c -p 1000` - show all system calls for PID
- `strace -t df -h` - show time of each system call
- `strace -e trace=open,read -p 1000 -o debug.txt` -  log a trace of the open, read system calls to file

