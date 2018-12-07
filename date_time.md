# Linux Server date and time

## Get current server datetime

```
date
```

## Set timezone

```
timedatectl set-timezone Asia/Yekaterinburg
```

## Show current timezone

```
ls -l /etc/localtime

Output:
  lrwxrwxrwx 1 root root 40 Dec  7 14:48 /etc/localtime -> ../usr/share/zoneinfo/Asia/Yekaterinburg
```

So file `/etc/localtime` is symlink to one of many system files in directory `/usr/share/zoneinfo`.

## Show available timezones

```
timedatectl list-timezones

Output:
  Asia/Yakutsk
  Asia/Yangon
  Asia/Yekaterinburg
    ...
```
