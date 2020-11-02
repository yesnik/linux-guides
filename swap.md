# Swap file

- `sudo swapon --show` - show info about current swap files
- `free -h` - show free memory, including swap

### Create swap file

1. Create a 2Gb file: `sudo dd if=/dev/zero of=/swapfile bs=1M count=2048`
    - `if` - input file
    - `of` - output file
    - `bs` - block size
    - `count` - how many blocks to copy
2. Make the file only accessible to root: `sudo chmod 600 /swapfile`
3. Mark the file as swap space: `sudo mkswap /swapfile`
4. Enable the swap file, allowing our system to start utilizing it: `sudo swapon /swapfile`
5. Verify that the swap is available: `sudo swapon --show`
6. If we reboot, the server will not retain the swap settings automatically. 
Add this line at the end of `/etc/fstab` file:
```
/swapfile none swap sw 0 0
```
