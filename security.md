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
## Firewall

- `sudo firewall-cmd --state` - check firewall status
- `sudo firewall-cmd --get-active-zones` - retrieve your currently active zones.
- `firewall-cmd --zone=public --permanent --add-service=https` - permanently open port 443 for service *https* (reload required)
- `sudo firewall-cmd --reload`
- `sudo firewall-cmd --list-all` - check for open ports/services. The services with permanently open ports are listed on line starting with *services*
- `sudo firewall-cmd --zone=public --permanent --remove-service=https` - close the previously open HTTP port 443

## Disable Root login

To disable login *root* user we need to create a user with sudo rights. Ensure that the new user can login to the server.

We are now going to disable root login, which means no one can ssh or log into the server as *root* user.

Edit `sshd` configuration file 

```
sudo vim /etc/ssh/sshd_conf
````

Edit this line:

```
PermitRootLogin no
```

After that restart the service:

```
service sshd restart
```

## Use a Non-Standard Port

By default, ssh listens for incoming connections on port 22. Hackers know this. 
It's better to pick some random high port (above 1024) that's not used for any known services. 
To make the change, edit file `/etc/ssh/sshd_config`:

```
# Run ssh on a non-standard port:
Port 2345  #Change me
```

Restart the sshd service: 

```
sudo service sshd restart
```

Don't forget to then make any necessary changes to port forwarding in your router and any applicable firewall rules. 
For example on CentOS 7 you need to change firewalld:

```
firewall-cmd --add-port 2345/tcp
firewall-cmd --add-port 2345/tcp --permanent
```

Or on CentOS 6:

```
iptables -I INPUT -p tcp --dport 2345 -j ACCEPT
```

On CentOS 6 and above you should also update selinux, labeling the chosen port correctly, 
otherwise sshd will be prevented from accessing it. For example:

```
semanage port -a -t ssh_port_t -p tcp 2345 #Change me 
```

Because ssh is no longer listening for connections on the standard port, you will need to tell your client what port to connect on. Using the ssh client from the command line, we may specify the port using the -p switch:

```
ssh -p 2345 myserver
```
