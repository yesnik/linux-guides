# Linux Processes

*Process* is a running instance of a program.

## Basic commands

- `ps -aux` - show info about processes. Params:
  * `-a` - all users
  * `-u` - additional info like `-f` option
  * `-x` - info about processes without terminals
  * `-f` - full info with process tree
  * `-e` - extended info with all args for each process
- `kill -9 500` - kill process with PID = 500

## Create process

A new process is normally created when an existing process makes an exact copy of itself in memory. 
The child process will have the same environment as its parent, but different PID.


