# Nginx Dot Files

It's better to secure `.env`, `.htaccess` files:

```nginx
location ~ /\.{
  deny all;
  access_log off;
  log_not_found off;
}
```
