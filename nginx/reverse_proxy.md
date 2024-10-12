# Reverse Proxy

Nginx can pass request to app server using different directives:

- `fastcgi_pass`, protocol `fastcgi` - to PHP-FPM
- `uwsgi_pass`, protocol `uWSGI` - to Python app
- `proxy_pass`, protocol `http` - to another host

## Proxy to another host

```nginx
server {
    listen 80;
    server_name mysite.com;

    location /hello {
        proxy_pass http://another.com/path;

        # We can add additional headers:
        proxy_set_header X-Real-ip $remote_addr;
        proxy_set_header X-Country-Code $geoip_country_code;
    }
}
```

- Visit http://mysite.com/hello
- You'll see the contents of page http://another.com/path
- Browser's address bar still shows http://mysite.com/hello

### Prevent passing header

To prevent a header field from being passed to the proxied server, set it to an empty string as follows:

```nginx
location /some/path/ {
    proxy_set_header Accept-Encoding "";
    proxy_pass http://localhost:8000;
}
```

## Proxy to many servers

```nginx
upstream backend {
    server app1.example.com;
    server app2.example.com;
}

server {
    listen 80;
    
    location / {
        proxy_pass http://backend;
    }
}
```
