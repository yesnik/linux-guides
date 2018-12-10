# Linux Security Tips

## Check Listening Ports

View open ports and and corresponding services:

```
netstat -tunlp
```

To install netstat on Centos: `yum install net-tools`

Disable the unwanted services from the system using ‘chkconfig’ command and close the ports that are not needed:

```
chkconfig serviceName off
```
