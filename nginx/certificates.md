# Install SSL certificates

```
server {
    listen 443 ssl http2;

    ssl_certificate /etc/nginx/ssl/domain.crt;
    ssl_certificate_key /etc/nginx/ssl/domain.key;
    
    # ...
}
```
