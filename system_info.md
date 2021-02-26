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

- `vmstat 5` - show info, update every 5 sec.
- `vmstat -s` - displays a table of various event counters  and  memory  statistics.
- `vmstat -w` - wide output mode (useful for systems with higher amount of memory)
