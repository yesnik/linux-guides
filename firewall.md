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


## iptables

### Restart iptables

```
sudo systemctl restart iptables

sudo service firewalld restart
```

### Show current iptables rules

```
iptables -L -n -v 
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
iptables -A INPUT -p tcp -m tcp --sport 80 -j ACCEPT
iptables -A OUTPUT -p tcp -m tcp --dport 80 -j ACCEPT
```
