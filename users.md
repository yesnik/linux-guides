# Linux Users

## Add user with home directory

```
adduser kenny
```

## Delete user including home directory

```
userdel -r kenny
```

## Change user's password

```
passwd kenny
```


## Show last login info

```
last

Output:
  admin pts/0          172.11.15.179    Fri Dec  7 15:08   still logged in
  our_app system boot  2.6.32-358.18.1. Thu Dec  6 22:03 - 15:09  (17:06)
  admin pts/0          172.11.15.179    Thu Dec  6 17:12 - down   (09:48)
```

## Show info about currend logged in users

```
w

Output:
 15:12:33 up 12:08,  1 user,  load average: 10.96, 9.83, 9.59
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
landingx pts/0    172.11.10.179     15:08    0.00s  0.16s  0.10s w
```

## Show current user name

```
whoami
```

## Show user's groups

### - current user

```
groups
```

### - user nginx

```
groups nginx

Output:
  nginx : nginx apache
```
