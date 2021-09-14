# Nginx File Rights

To fix permission error: *PHP Warning:  Unknown: failed to open stream: Permission denied in Unknown on line 0*

```bash
chown -R "$USER":nginx /var/www/html/sales
chmod -R 2755 /var/www/html/sales
```
