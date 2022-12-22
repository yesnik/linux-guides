# System boot

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
service kafka-update-crm-status start
service kafka-update-crm-status status

# Read logs
journalctl -u kafka-update-crm-status
```
