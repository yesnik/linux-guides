# Linux Network

- `tcpdump -i eth0` - show traffic on a network

## Network kernel params

`sysctl net.core` - show network kernel params

Kernel params description:

- `net.core.somaxconn = 20000` - socket max connections. Default 128, Max 65535. It's the max number of connection requests that can be queued for any given listening socket.
- `net.core.netdev_max_backlog = 65535` - the maximum number of packets, queued on the INPUT side, when the interface receives packets faster than kernel can process them.
