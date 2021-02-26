# Linux system info

- `cat /etc/*-release` - show Linux version
- `fdisk -l` - List  the  partition  tables for the specified devices and then exit.  If no devices are given, those mentioned in /proc/partitions (if that exists) are used
- `hostnamectl` - show Linux version
- `uname -sr` - show kernel version
- `lscpu` - info about CPU
- `cat /proc/cpuinfo` - info about CPU
- `cat /etc/services` - port assignments to network services
- `htop --tree` - show processes with tree
- `iptstate` - displays information held in the IP Tables state table in real-time in a top-like format
- `sysctl -a` - show current *sysctl* settings
- `sysctl -w net.core.somaxconn=65535` - set setting immediately (not persistent)
- `sudo echo "net.core.somaxconn = 65535" | tee -a /etc/sysctl.conf && sudo sysctl -p` - set setting (persistent)
- `cat /proc/sys/net/core/somaxconn` - show current value of setting
- `sudo swapon --show` - show info about current swap files
- `free -h` - show free memory, including swap

## systemctl

- `systemctl list-units` - list active modules of systemd
- `systemctl list-unit-files` - list status of all available modules

## vmstat

It reports information about processes, memory, paging, block IO, traps, disks and cpu activity.
These reports are intended to help identify system bottlenecks.

- `vmstat 2` - show info, update every 2 sec.
- `vmstat -s` - displays a table of various event counters  and  memory  statistics.
- `vmstat -w` - wide output mode (useful for systems with higher amount of memory)
- `vmstat -f` - show number of forks. It shows the number of tasks that have been launched (and, for the bulk of them, closed again) since the system was booted. Every process launched from the command line would increase this figure. Each time a task or process spawns or clones a new task, this figure will increase.

**Output**

```
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 4  0 100864  22992      0 219372    0    2     9    12   51   75  1  1 99  0  0
```

- Procs
    - `r`: The number of processes waiting for run time by CPU. If r > 0, then CPU is overloaded
    - `b`: The number of processes in uninterruptible sleep. If b > 0, then disks / filesystem is overloaded
- Memory
    - `swpd`: the amount of virtual memory used.
    - `free`: the amount of idle memory.
    - `buff`: the amount of memory used as buffers.
    - `cache`: the amount of memory used as cache.
    - `inact`: the amount of inactive memory. (-a option)
    - `active`: the amount of active memory. (-a option)
- Swap
    - `si`: Amount of memory swapped in from disk (/s).
    - `so`: Amount of memory swapped to disk (/s).
- IO
    - `bi`: Blocks received from a block device (blocks/s).
    - `bo`: Blocks sent to a block device (blocks/s).
- System
    - `in`: The number of interrupts per second, including the clock.
    - `cs`: The number of context switches per second.
- CPU
    - `us`: Time spent running non-kernel code. (user time, including nice time)
    - `sy`: Time spent running kernel code. (system time)
    - `id`: Time spent idle. Prior to Linux 2.5.41, this includes IO-wait time.
    - `wa`: Time spent waiting for IO. Prior to Linux 2.5.41, included in idle.
    - `st`: Time stolen from a virtual machine. Prior to Linux 2.6.11, unknown.
