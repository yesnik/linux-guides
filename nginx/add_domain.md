# Add domain

We can register many websites on one IP address. Let's allow your server to handle requests for specific domain name.

Create `/etc/nginx/conf.d/nikos.ru.conf`:

```
server {
    listen 80;
    root /var/www/nikos.ru;
    index index.html index.htm;
    server_name nikos.ru;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
**Important:** This config file must have `.conf` extension.

To apply config changes reload nginx: `sudo service nginx reload`

## Add subdomain

We can display another site for subdomain. Create `/etc/nginx/conf.d/news.nikos.ru.conf`:

```
server {
    listen 80;
    root /var/www/news.nikos.ru;
    index index.html index.htm;
    server_name news.nikos.ru;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Reload nginx.
