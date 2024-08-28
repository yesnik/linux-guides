# Upstream Nginx

See [upstream docs](https://nginx.org/ru/docs/http/ngx_http_upstream_module.html)

Nginx can proxy requests to several servers:

```nginx
upstream backend {
    server 127.0.0.1:8085;
    server 127.0.0.1:8086;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;

        proxy_set_header X-Real-ip $remote_addr;
        proxy_set_header X-Country-Code $geoip_country_code;

        proxy_connect_timeout 60s;
        proxy_read_timeout 60s;
        proxy_send_timeout 60s;
    }
}

server {
    listen 127.0.0.1:8085;

    server_name _;

    location / {
        return 200 "8085";
    }
}

server {
    listen 127.0.0.1:8086;

    server_name _;

    location / {
        return 200 "8086";
    }
}
```
Ensure that Nginx returns responses from 2 backend servers:

```bash
curl -D - http://example.com/internal
```

