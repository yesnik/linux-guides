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

## Disable Root login

Create a user with sudo rights so that we can ssh into the server and perform administrative tasks.

### Centos

Create new user:

```
adduser myuser
```

Create password for the new user:

```
passwd myuser
```

Provide sudo permissions to the newly added user. On Centos add user to `wheel` group to give it sudo privileges:

```
usermod -aG wheel username
```

On another linux:

```
echo 'myuser ALL=(ALL) ALL' >> /etc/sudoers
```

SSH to the server with the new user and ensure that the login works.

**Important**: Don’t log out of the server yet. First test whether you can successfully ssh into the server using the previously created user. Open another instance of the terminal and ssh into the server with user you previously created. 
If everything works fine, you can safely log out of the server as root.

We are now going to disable root login, which means no one can ssh or log into the server as root user. 
Edit sshd configuration file `/etc/ssh/sshd_conf`:

```
PermitRootLogin no
```

After that restart the service:

```
service sshd restart
```

