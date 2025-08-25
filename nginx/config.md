# Nginx config

Official docs: https://nginx.org/en/docs/

## Config sections

Main `/etc/nginx/nginx.conf` config file has different sections:

```nginx
# Main section
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

# Events section
events {
    worker_connections  1024;
}

# HTTP section
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    keepalive_timeout  65;
    include /etc/nginx/conf.d/*.conf;
}
```

### Location section

General description for location:

```nginx
location <modifier> <prefix> {
    # ...
}
```

#### No modifier

```nginx
location /upload/ {
    # Any query beginning with /upload/ but the process continues searching.
    # Will only be matched if regular expressions don't find a match.
}
```

#### Modifier `=`

The equal sign can be used if the location needs to match the exact request URI. When this modifier is matched, the search stops right here.

In the next example:

- `/app/info` - will return 200 (exact match)
- `/app/info/1` - will return 201

```nginx
server {
  listen 80;
  server_name hello.com;

  location = /app/info {
    return  200;
  }
  location /app {
    return 201;
  }
}
```

Test:

```bash
# Will return 200
curl -v http://hello.com/app/info

# Will return 201
curl -v http://hello.com/app/info/some
curl -v http://hello.com/app/hey
curl -v http://hello.com/appa

# Will return 404
curl -v http://hello.com/some/app
curl -v http://hello.com/some/APP
```

#### Modifier `~`

Case sensitive RegExp. It has more priority than location without modifiers:

- `/app/file.txt` - will return 201
- `/app/info.TXT` - will return 202

```nginx
server {
  listen 80;
  server_name hello.com;

  # It has less priority than location with modifier
  location /app/ {
    return 200;
  }

  location ~ \.txt$ {
    return 201;
  }

  location ~ \.TXT {
    return 202;
  }
}
```

Test:

```bash
# Will return 404
curl -v -H "Host: hello.com" http://95.91.69.190/app
curl -v -H "Host: hello.com" http://95.91.69.190/some.Txt
# Will return 200
curl -v -H "Host: hello.com" http://95.91.69.190/app/
# Will return 201
curl -v -H "Host: hello.com" http://95.91.69.190/some.txt
# Will return 202
curl -v -H "Host: hello.com" http://95.91.69.190/some.TXT
```
Example with PHP files:

```nginx
location ~ \.php$ {
    try_files $uri /index.php =404;
    fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
    fastcgi_index index.php;
    fastcgi_buffers 16 16k;
    fastcgi_buffer_size 32k;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_read_timeout 600;
    include fastcgi_params;
}
```

#### Modifier `~*`

It's case insensitive RegExp modifier.

#### Modifier `^~`

Assuming this block is the best non-RegExp match, a carat followed by a tilde modifier means that RegExp matching will not take place.

```nginx
location ^~ /assets/ {
    # Queries beginning with /assets/ and then stops searching
}

location ^~ /api/v1/products {
    proxy_pass http://k8s-prod.site.com:30130/api/v1/products;
}
```

#### Named location `@abc`

It's internal nginx location, we can't invoke it like this: `http://hello.com/app`

```nginx
location @app {
    proxy_pass http://backend;
}

location / {
    try_files /maintenance.html $uri $uri/ @app;
}
```

## Config options

### ngx_core_module

- See [docs](https://nginx.org/en/docs/ngx_core_module.html)

- `worker_processes auto;` - Defines the number of worker processes. Setting it to the number of available CPU cores would be a good start (the value "auto" will try to autodetect it).

### ngx_http_core_module

See [docs](https://nginx.org/en/docs/http/ngx_http_core_module.html).

- `client_max_body_size 32m;` - Sets the maximum allowed size of the client request body
- `listen: 80 default_server;` - The `default_server` parameter, if present, will cause the server to become the default server for the specified `address:port` pair.

### ngx_http_log_module

See [docs](https://nginx.org/en/docs/http/ngx_http_log_module.html)

It writes request logs in the specified format.

We can define our own log format with name "combined":

```nginx
log_format combined '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
```
After that we can use it:

```nginx
location / {
    access_log /var/log/nginx/access.index.log combined;
}
```

We can define logging level in the error log:

```nginx
location / {
    error_log /var/log/nginx/error.index.log debug;
}
```

## Nginx config examples

### Simple virtual host

File `/etc/nginx/conf.d/hello.com.conf`:

```nginx
server {
    listen 80;
    server_name hello.com;
    return 200 "From hello.com";
}
```
Test:

```
curl -v -H "Host: hello.com" http://95.91.69.190/
```

### Two sites on a single domain

File `/etc/nginx/conf.d/site.conf`:

```nginx
server {
    listen 80;
    root /var/www/html;
    index index.html;
    server_name mysite.com;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Create 2 files:

```
echo "site1" > /var/www/html/site1/index.html
echo "site2" > /var/www/html/site2/index.html
```

Visit:

- `http://mysite.com/site1` - result: mysite1
- `http://mysite.com/site2` - result: mysite2

### Two domains on a single Nginx server

File `mysite1.conf`:

```nginx
server {
   listen 80;
   root /var/www/html/mysite1.com;
   index index.html;
   server_name mysite1.com;

   location / {
       try_files $uri $uri/ =404;
   }
}
```

File `mysite2.conf`:

```nginx
server {
   listen 80;
   root /var/www/html/mysite2.com;
   index index.html;
   server_name mysite2.com;

   location / {
       try_files $uri $uri/ =404;
   }
}
```

### PHP app (1)

File `/etc/nginx/conf.d/hello.com.conf`:

```nginx
server {
    listen 80;
    server_name hello.com;
    charset utf-8;
    index index.php index.html;
    root /app/public;

    add_header X-Frame-Options "SAMEORIGIN";

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;

        # Way 1. Via Unix Domain Sockets
        fastcgi_pass api-php:9000;
        # Way 2. Via TCP Sockets
        fastcgi_pass unix:/run/php-fpm/www.sock;

        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

### PHP app (2)

File `/etc/nginx/conf.d/qm.hello.com.conf`:

```nginx
server {
    listen 80;
    server_name qm.hello.com;
    root /var/www/qm.hello.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

### PHP app with SSL

File `/etc/nginx/conf.d/stage.hello.ru.conf`:

```nginx
server {
    listen 80;

    server_name  stage.hello.ru;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/stage.hello.ru/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/stage.hello.ru/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

    client_max_body_size 32m;
    server_name  stage.hello.ru;
    root   /var/www/hello;
    index index.html index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ ^/(protected|framework|themes/\w+/views) {
        deny  all;
    }

    location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
        try_files $uri =404;
    }

    location ~ \.php {
        root /var/www/hello;
        fastcgi_index   index.php;
        fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
        include fastcgi_params;
    }

    location ~ /\. {
        deny all;
        access_log off;
        log_not_found off;
    }
}
```
