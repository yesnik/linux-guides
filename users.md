# Linux Users

OS: Centos, Ubuntu

## Add user

### adduser

It's a friendlier front end to the low level tool `useradd`.

```bash
adduser kenny
```

Folder `/home/kenny` will be created.

**Add group 'app' and user 'app':**

```bash
# -G GRP	Group
# -D		  Don't assign a password
# -H		  Don't create home directory
addgroup app && adduser -DH -G app app
```

**Create a Non-login User**

```bash
# Way 1. Home directory '/home/prometheus' will be created
sudo adduser kenny --system
# Way 2. Create home dir, user, group, Full Name, Room number, etc.
sudo adduser kenny --shell /usr/sbin/nologin
```
After this we cannot login as this user:

```bash
su - kenny
# This account is currently not available.
```

### useradd

`useradd` is native binary compiled with the system.

```bash
# Way 1
useradd -s /bin/bash -m kenny

# Way 2. Create a Non-login User
useradd -s /sbin/nologin -d /opt/prometheus prometheus
```

Options:
- `-s` - specify the new user's login shell. Default values specified in the `/etc/default/useradd` file.
- `-m` - create the user home directory as `/home/kenny`
- `-d` - home directory of the new account

## Delete user

```
userdel -r kenny
```

Folder `/home/kenny` will be removed.

## Change user's password

```
passwd kenny
```

## Login as user

```bash
su - kenny
```

## Add user to group

Add user *kenny* to group *nginx*:

```
sudo usermod -aG nginx kenny
```

**Important:** If the group was just created the user must re-login in order for the group's permissions to be applied.

## Add user to sudoers

To do so you need to edit the `/etc/sudoers` this editor:

```
sudo visudo
```

### Centos

On CentOS, members of the *wheel* group have sudo privileges.

```
usermod -aG wheel kenny
```

To allow users from group *wheel* to run all commands using `sudo` without a password, edit the following line:

```
## Same thing without a password
%wheel  ALL=(ALL)       NOPASSWD: ALL
```

### Ubuntu

On Ubuntu members of the *sudo* group have sudo privileges.

```
usermod -aG sudo kenny
```

In case you need all members of the *sudo* group to execute any commands using *sudo* withoud password, change the config line:

```
# Allow members of group sudo to execute any command
# %sudo   ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
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

## Show current user info

- `whoami` - show current user name
- `id` - show uid, gid, groups of current user
- `id -u`, `echo $UID` - show user id
- `id -g` - show user's group id

## Show user info

- `id -nu 1000` - show name of user with uid=1000

## Show user's groups

- `getent group | cut -d: -f1` - show all groups
- `groups` - show groups for current user
- `groups nginx` - show groups of user nginx

## Show all users

```
# Way 1
cat /etc/passwd
# Way 2
getent passwd | cut -d: -f1
# Way 3
awk -F: '{ print $1}' /etc/passwd
# Way 4
cut -d: -f1 /etc/passwd
```

*Note*: Users with shell `/usr/sbin/nologin` are restricted from logging into our server.

## Run command as another user

Run command `ls /var/www/sales` as user `sales`:

```bash
runuser -u sales -- ls /var/www/sales
```
