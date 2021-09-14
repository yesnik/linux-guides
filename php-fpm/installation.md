# PHP FPM Installation

## Centos

- Install php-fpm:
  ```bash
  yum install php-fpm
  ```
- Start service php-fpm
  ```bash
  service start php-fpm
  ```
- Enable php-fpm autostart on system boot
  ```bash
  systemctl enable php-fpm
  # Created symlink from /etc/systemd/system/multi-user.target.wants/php-fpm.service to /usr/lib/systemd/system/php-fpm.service.
  ```
