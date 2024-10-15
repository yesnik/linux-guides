# SELinux

## OS Centos 7

### Disable SELinux

```bash
setenforce 0
```

### Config SELinux

The setroubleshoot packages provide tools to help diagnose SELinux problems:

```bash
yum install setroubleshoot-server -y
```

Show a report on denials:

```bash
aureport -a # 
```

Show log of user login:
```bash
ausearch -m user_login -ts recent # last 10 min
```

Read input from audit log:

```bash
audit2allow -a
```

Create policy:
```bash
audit2allow -a -M read
```

Make new policy active:

```bash
semodule -i read.pp
```
