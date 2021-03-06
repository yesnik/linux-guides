# System boot

## Ubuntu

If you want to edit services started at system startup use following commands.

**Add service to startup**

```
sudo update-rc.d apache2 defaults
```

This command will create the symlink `/etc/rc0.d/K01apache2` to `/etc/init.d/apache2`.

**Remove service from startup**

```
sudo update-rc.d apache2 remove
```

This command will remove symlink from `/etc/rc0.d/K01apache2` to `/etc/init.d/apache2`.
It means that `apache2` will no longer be booted on system startup.

## Centos 7

### Add service to startup

**Method 1**

Enable it to start Docker automatically on boot:

```bash
sudo systemctl enable docker

# Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.
```

**Method 2**

If you want to automatically start `php-fpm` on a system boot:

```bash
chkconfig php-fpm on

# Note: Forwarding request to 'systemctl enable php-fpm.service'.
# Created symlink from /etc/systemd/system/multi-user.target.wants/php-fpm.service 
# to /usr/lib/systemd/system/php-fpm.service.
```
