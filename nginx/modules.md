# Nginx Modules

Modules list: see "Modules reference" on the page: https://nginx.org/en/docs/

Show installed nginx modules: 

- `nginx -V`
- `dpkg -l | grep nginx`

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
        add_header X-Country3 $geoip_country_code3;
        add_header X-CountryName $geoip_country_name;

        return 200;
    }
}
```
Test: `curl -vvv http://site.com/`

In the response we will see headers:

```
X-Country: RU
X-Country3: RUS
X-CountryName: Russian Federation
```

## brotli

Helps to compress js, css.

See https://github.com/google/ngx_brotli

## vts

Nginx virtual host traffic status module. 

See https://github.com/vozlt/nginx-module-vts

## nginx-auth-ldap

LDAP authentication module for nginx.

See https://github.com/kvspb/nginx-auth-ldap

## nginx_upstream_check_module

It adds health checks for servers in upstreams.

See https://github.com/yaoweibin/nginx_upstream_check_module
