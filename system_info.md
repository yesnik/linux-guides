# Linux system info

- `cat /etc/*-release` - show Linux version
- `hostnamectl` - show Linux version
- `uname -sr` - show kernel version
- `lscpu` - info about CPU
- `cat /proc/cpuinfo` - info about CPU
- `cat /etc/services` - port assignments to network services
- `htop --tree` - show processes with tree
- `sysctl -a` - show current *sysctl* settings
- `sysctl -w net.core.somaxconn=65535` - set setting immediately (not persistent)
- `sudo echo "net.core.somaxconn = 65535" | tee -a /etc/sysctl.conf && sudo sysctl -p` - set setting (persistent)
- `cat /proc/sys/net/core/somaxconn` - show current value of setting
- `sudo swapon --show` - show info about current swap files
- `free -h` - show free memory, including swap
