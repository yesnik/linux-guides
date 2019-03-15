# iptables

## Centos 7

### Restart iptables

```
systemctl restart iptables
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
