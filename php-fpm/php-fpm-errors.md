# PHP-FPM Errors

`tail -200 -f /var/log/php-fpm/errors.log`

### WARNING: [pool www] seems busy

Full text of error: *WARNING: [pool www] seems busy (you may need to increase pm.start_servers, 
or pm.min/max_spare_servers) spawning 32 children, there are 0 idle, and 46 total children*

This error occurs when too many users try to hit our site.

Edit these params in config `/etc/php-fpm/php-fpm.conf`:

```
pm = ondemand
pm.max_children = 1024 
pm.process_idle_timeout = 10s
```
