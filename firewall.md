# Centos Firewall

## firewall-cmd

CentOS 8 comes with a dynamic, customizable host-based firewall with a D-Bus interface. 
You can add or delete or update firewall rules without restarting the firewall daemon or service. 
The `firewall-cmd` acts as a frontend for the `nftables`. In CentOS 8 `nftables` replaces `iptables` as the default Linux network packet filtering framework.

### Commands

- `sudo firewall-cmd --state` - check status
- `sudo systemctl start firewalld` - start
- `sudo systemctl enable firewalld` - enable
- `firewall-cmd --get-zones` - get all zones
- `firewall-cmd --get-default-zone`
- `sudo firewall-cmd --list-all` - list firewall rules
- `sudo firewall-cmd --reload` - apply changes in rules
- `sudo firewall-cmd --add-service http` - allow HTTP connections on port 80
- `sudo firewall-cmd --remove-service=http --permanent` - restrict connection to port 80
- `sudo firewall-cmd --permanent --add-port=80/tcp`
- `sudo firewall-cmd --permanent --remove-port=80/tcp`

### Configuration for docker

```bash
# trust the docker interface
firewall-cmd --permanent --zone=trusted --change-interface=docker0
# accept IPv4 traffic
firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 4 -i docker0 -j ACCEPT
# any ports on the host you want to access from the containers (strapi port 1337 here)
firewall-cmd --permanent --zone=trusted --add-port=1337/tcp
firewall-cmd --reload
service docker restart
```

## iptables

### Restart iptables

```
sudo systemctl restart iptables
```

### Show current iptables rules

```
iptables -L -v -n --line-num
```

### Deny connections from IP

```bash
sudo iptables -A INPUT -s 155.10.10.55 -j DROP
```

### Allow connections from IP to port 3306

Add line to file `/etc/sysconfig/iptables`:

```
# Allow connections for company Hello.com
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -s 200.120.60.10 -j ACCEPT
```

To apply changes restart: `systemctl restart iptables`

### Open port 80

```
iptables -I INPUT -p tcp -m tcp --sport 80 -j ACCEPT
iptables -I OUTPUT -p tcp -m tcp --dport 80 -j ACCEPT
```
Here:
- `-I` - insert to the beginning of the list of rules
- `-A` - append to the end
