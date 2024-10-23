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

### Test

Ensure that Nginx returns responses from 2 backend servers:

```bash
curl -D - http://example.com/internal
```

### Server offline

If one of the servers in the backend upstream offline, Nginx will route all requests to available server.

## weight

From 3 requests we can ask Nginx to pass two requests to server 1 and one request to server 2:

```nginx
upstream backend {
    server 127.0.0.1:8085 weight=2;
    server 127.0.0.1:8086 weight=1;
}
```
