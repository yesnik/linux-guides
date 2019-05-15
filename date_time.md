# Linux Server date and time

## Get current server datetime

```
date
```

## Set timezone

**Centos / Ubuntu**

```
sudo timedatectl set-timezone Asia/Yekaterinburg
```

**Ubuntu**

```
echo "Asia/Yekaterinburg" > /etc/timezone
dpkg-reconfigure -f noninteractive tzdata
```

## Show current timezone

**Centos**

```
ls -l /etc/localtime

Output:
  lrwxrwxrwx 1 root root 40 Dec  7 14:48 /etc/localtime -> ../usr/share/zoneinfo/Asia/Yekaterinburg
```

So file `/etc/localtime` is symlink to one of many system files in directory `/usr/share/zoneinfo`.

**Ubuntu**

```
cat /etc/timezone 

Output:
Asia/Yekaterinburg
```

## Show available timezones

```
timedatectl list-timezones

Output:
  Asia/Yakutsk
  Asia/Yangon
  Asia/Yekaterinburg
    ...
```
