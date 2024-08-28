# Upstream Nginx

Nginx can proxy requests to several servers:

```nginx
upstream backend {
    server app1.example.com weight=1;
    server app2.example.com weight=2;
    
    server app3.example.com backup;
}

server {
    listen 80;
    
    location / {
        proxy_pass http://backend;

        proxy_set_header X-Real-ip $remote_addr;
        proxy_set_header X-Country-Code $geoip_country_code;

        proxy_connect_timeout 60s;
        proxy_read_timeout 60s;
        proxy_send_timeout 60s;
    }
}
```
