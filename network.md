# Linux Network

- `tcpdump -i eth0` - show traffic on a network
- `ip a` - show network settings, IP address of current server
- `ifconfig` - show network interfaces
- `ifconfig venet0:2 down` - disable interface named `venet0:2`
- `route -n` - show IP routing table
- `traceroute google.com` - elicit a response from the router at each hop from your computer to the destination
- `telnet 127.0.0.1 3000` - check connection to host and port
- `nc -zv 127.0.0.1 3000` - check connection to host and port
- `netstat -tnlp` - show open ports. Install package: `apt install net-tools`
- `ss -rt` - show opened TCP sockets.
- `dig +short youtube.com` - show all IP addresses of domain 

## Restart network

```
systemctl restart NetworkManager
```

## tcpdump

- `tcpdump -n host google.com` - capture connections to host
- `tcpdump port 80` - see only traffic to or from a certain port
- `tcpdump -n portrange 80-150` - traffic in the range of ports
- `tcpdump -n dst port 80` - trafic from any source to destination port 80
- `tcpdump -n udp` - capture UDP traffic
- `tcpdump -D` - show available network interfaces
- `tcpdump -ni any` - capture packets from all interfaces
- `tcpdump -c 10` - capture first 10 packets

**Options**

- `-n` - Don't convert addresses (i.e., host addresses, port numbers, etc.) to names

**Output format**

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

## CIDR

**CIDR** is the short for Classless Inter-Domain Routing, an IP addressing scheme. A single IP address can be used to designate many unique IP addresses with CIDR. A CIDR IP address looks like a normal IP address except that it ends with a slash followed by a number, called the IP network prefix. CIDR addresses reduce the size of routing tables.

- `10.0.0.0/24` - First IP `10.0.0.0`, Last IP `10.0.0.255`
- `172.16.0.0/12` - First IP `172.16.0.0`, Last IP `172.31.255.255`
- `177.17.0.0/21` - First IP `177.17.0.0`, Last IP `177.17.7.255`

See: [CIDR to IPv4 Convertor](https://www.ipaddressguide.com/cidr)
