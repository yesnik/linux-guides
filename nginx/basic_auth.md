# Nginx HTTP Basic Authentication

1. Verify that `apache2-utils` (Debian, Ubuntu) or `httpd-tools` (RHEL/CentOS/Oracle Linux) is installed.

2. Create file with password for user `kenny`:

```bash
sudo htpasswd -c /var/www/mysite/.htpasswd kenny
```
**Note:** If you want to add password for another user, use existing file with passwords:

```bash
sudo htpasswd /var/www/mysite/.htpasswd leo
```

3. If we want to secure `/status` URL with Basic auth, add to `/etc/nginx/nginx.conf`:

```nginx
http {
    # ...
    server {
        # ...

        location /status {
            auth_basic_user_file /var/www/mysite/.htpasswd;
            auth_basic "Password-protected Area";
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
            
            # Define here correct path to php-fpm socket
            fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        }
    }
}
```
