# Nginx Modules

Modules list: see "Modules reference" on the page: https://nginx.org/en/docs/

## ngx_http_geoip_module

See [docs](https://nginx.org/en/docs/http/ngx_http_geoip_module.html)

Edit `nginx.conf`:

```nginx
http {
    geoip_country /usr/share/GeoIP/GeoIP.dat;
    # ...
}
```

### Return header with country code

```nginx
server {
    listen 80;
    
    location / {
        add_header X-Country $geoip_country_code always;
        return 200;
    }
}
```
