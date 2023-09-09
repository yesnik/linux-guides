# System boot

## Run a command on startup in Linux

### Crontab

Put the command in your crontab file. Run command `sudo crontab -e` to open your crontab file.
At the first available line, type "@reboot xxxx", where "xxxx" is the command you wish to run. Save the file and exit.

### Add systemd service

Add file `/etc/systemd/system/restore-iptables-rules.service`

```
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/bin/bash /home/nik/scripts/restore-iptables-rules.service

[Install]
WantedBy=multi-user.target
```
File `/home/nik/scripts/restore-iptables-rules.service`:

```
sudo /usr/sbin/iptables-restore < /etc/sysconfig/iptables
```

Enable this service:

```bash
systemctl enable restore-iptables-rules.service
```

## Add / Remove service to startup

If you want to edit services started at system startup use following commands.

### Ubuntu

**Method 1**

```bash
sudo systemctl status apache2
sudo systemctl enable apache2
sudo systemctl disable apache2
```

**Method 2**

```
sudo update-rc.d apache2 defaults

sudo update-rc.d apache2 remove
```

This command will create / remove the symlink `/etc/rc0.d/K01apache2` to `/etc/init.d/apache2`.

### Centos 7

**Method 1**

Start Docker automatically on boot:

```bash
sudo systemctl enable docker

# Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.
```

Disable Docker start on boot:

```bash
sudo systemctl disable docker
# Removed /etc/systemd/system/multi-user.target.wants/docker.service.

```

**Method 2**

If you want to automatically start `php-fpm` on a system boot:

```bash
chkconfig php-fpm on

# Note: Forwarding request to 'systemctl enable php-fpm.service'.
# Created symlink from /etc/systemd/system/multi-user.target.wants/php-fpm.service 
# to /usr/lib/systemd/system/php-fpm.service.
```

**Method 3**

Create file `/usr/lib/systemd/system/kafka-update-crm-status.service`:

```
[Unit]
Description=Kafka Update CRM status
After=network.target

[Service]
User=nginx
Group=sales
Restart=always
RestartSec=100
ExecStart=/usr/bin/php /var/www/mysite/current/protected/yiic.php kafkaMessagesListener

[Install]
WantedBy=multi-user.target
```

**Note:** Ensure that you defined correct values for `User`, `Group`.

Commands:

```bash
# Reload config. It's safe to run on production
systemctl daemon-reload

service kafka-update-crm-status start
service kafka-update-crm-status status

# Add script to autoload
systemctl enable kafka-update-crm-status

# Read logs
journalctl -u kafka-update-crm-status
```
