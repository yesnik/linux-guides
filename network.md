# Linux Network

- `tcpdump -i eth0` - show traffic on a network
- `ip a` - show network settings, IP address of current server
- `ifconfig` - show network interfaces
- `ifconfig venet0:2 down` - disable interface named `venet0:2`

## tcpdump

- `tcpdump -n host google.com` - capture connections to host
- `tcpdump port 80` - see only traffic to or from a certain port
- `tcpdump -n portrange 80-150` - traffic in the range of ports
- `tcpdump -n udp` - capture UDP traffic
- `tcpdump -D` - show available network interfaces
- `tcpdump -ni any` - capture packets from all interfaces
- `tcpdump -c 10` - capture first 10 packets

### Output format

```
06:41:02.633729 IP 37.230.124.116.34194 > 77.88.55.55.80: Flags [P.], seq 1:75, ack 1, win 502, 
options [nop,nop,TS val 526238602 ecr 3869570736], length 74: HTTP: POST / HTTP/1.1
```
- `06:41:02.633729` - hours:minutes:seconds.frac. frac - number of seconds after midnight
- `IP` - protocol IPv4
- `37.230.124.116.34194` - source host and port
- `77.88.55.55.80` - destination host and port
- `Flags` - TCP flags. To remember easier: *Unskilled Attackers Pester Real Security Folks*
    - Unskilled `URG` (Not Displayed in Flag Field, Displayed elsewhere) 
    - Attackers `ACK` (Not Displayed in Flag Field, Displayed elsewhere)
    - Pester `PSH` [P] (Push Data)
    - Real `RST` [R] (Reset Connection)
    - Security `SYN` [S] (Start Connection)
    - Folks `FIN` [F] (Finish Connection)
    - `SYN-ACK` [S.] (SynAcK Packet)
    - [.] (No Flag Set)
- `seq 1:75` - packet contains bytes from 1 to 75 of the data flow
- `ack 1` - serial number of next slice of data that destination host is waiting for
- `win 502` - window, amount of available bytes in buffer
- `options [nop,nop,TS val 526238602 ecr 3869570736]`
- `length 74` - payload length

## Network kernel params

`sysctl net.core` - show network kernel params

Kernel params description:

- `net.core.somaxconn = 20000` - socket max connections. Default 128, Max 65535. It's the max number of connection requests that can be queued for any given listening socket.
- `net.core.netdev_max_backlog = 65535` - the maximum number of packets, queued on the INPUT side, when the interface receives packets faster than kernel can process them.
- `net.ipv4.tcp_max_orphans = 2048` - maximum number of TCP sockets not attached to any user file handle, held by system. If this number is exceeded orphaned connections are reset immediately and warning is printed. This limit exists only to prevent simple DoS attacks, you must not rely on this or lower the limit artificially, but rather increase it (probably, after increasing installed memory), if network conditions require more than default value, and tune network services to linger and kill such states more aggressively. Note: each orphan eats up to ~64K of unswappable memory.
- `net.ipv4.tcp_max_syn_backlog = 65535` - maximum number of remembered connection requests, which are still did not receive an acknowledgment from connecting client. Default 1024 (if RAM 128Mb), 128 (if RAM < 128Mb).
