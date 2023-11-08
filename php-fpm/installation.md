# PHP FPM Installation

## Ubuntu

- Install php-fpm: `sudo apt install php-fpm`
- Check status: `service php8.1-fpm status`

## Centos

- Install php-fpm: `yum install php-fpm`
- Start service php-fpm: `service start php-fpm`
- Enable php-fpm autostart on system boot
  ```bash
  systemctl enable php-fpm
  # Created symlink from /etc/systemd/system/multi-user.target.wants/php-fpm.service to /usr/lib/systemd/system/php-fpm.service.
  ```
