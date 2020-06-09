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

## Swap file

- `sudo swapon --show` - show info about current swap files
- `free -h` - show free memory, including swap

### Create swap file

1. Create a 2Gb file: `sudo dd if=/dev/zero of=/swapfile bs=1M count=2048`
2. Make the file only accessible to root: `sudo chmod 600 /swapfile`
3. Mark the file as swap space: `sudo mkswap /swapfile`
4. Enable the swap file, allowing our system to start utilizing it: `sudo swapon /swapfile`
5. Verify that the swap is available: `sudo swapon --show`
6. If we reboot, the server will not retain the swap settings automatically. 
Add this line at the end of `/etc/fstab` file:
```
/swapfile none swap sw 0 0
```
