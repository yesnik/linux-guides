# Linux Network

- `tcpdump -i eth0` - show traffic on a network
- `ip address show`, or `ip a s` - show IP address of current server

## Network kernel params

`sysctl net.core` - show network kernel params

Kernel params description:

- `net.core.somaxconn = 20000` - socket max connections. Default 128, Max 65535. It's the max number of connection requests that can be queued for any given listening socket.
- `net.core.netdev_max_backlog = 65535` - the maximum number of packets, queued on the INPUT side, when the interface receives packets faster than kernel can process them.
- `net.ipv4.tcp_max_orphans = 2048` - maximum number of TCP sockets not attached to any user file handle, held by system. If this number is exceeded orphaned connections are reset immediately and warning is printed. This limit exists only to prevent simple DoS attacks, you must not rely on this or lower the limit artificially, but rather increase it (probably, after increasing installed memory), if network conditions require more than default value, and tune network services to linger and kill such states more aggressively. Note: each orphan eats up to ~64K of unswappable memory.
- `net.ipv4.tcp_max_syn_backlog = 65535` - maximum number of remembered connection requests, which are still did not receive an acknowledgment from connecting client. Default 1024 (if RAM 128Mb), 128 (if RAM < 128Mb).
