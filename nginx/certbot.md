# HTTPS with Certbot

If you want add https to the site, use [Certbot](https://certbot.eff.org/).

## Automatic installation

Generate certificate for your domain:

```bash
sudo certbot --nginx
```

Certbot will ask you about desired domain and will modify nginx config files.

## Manual installation

Generate and register certificates for your domain:

```bash
certbot certonly
```
This command will create files:

- Certificate and chain: `/etc/letsencrypt/live/mysite.com/fullchain.pem`
- Key file: `/etc/letsencrypt/live/mysite.com/privkey.pem`

Edit Nginx config `/etc/nginx/sites-enabled/default`:

```nginx
server {
    listen 80;

    server_name mysite.com;

    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl;

    server_name mysite.com;

    index index.html;
    root /var/www/mysite.com;

    ssl_certificate /etc/letsencrypt/live/mysite.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/mysite.com/privkey.pem;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
