# Nginx config

Official docs: https://nginx.org/en/docs/

## Config options

### ngx_core_module

- See [docs](https://nginx.org/en/docs/ngx_core_module.html)

- `worker_processes auto;` - Defines the number of worker processes. Setting it to the number of available CPU cores would be a good start (the value "auto" will try to autodetect it).

### ngx_http_core_module

See [docs](https://nginx.org/en/docs/http/ngx_http_core_module.html).

- `client_max_body_size 32m;` - Sets the maximum allowed size of the client request body

## Nginx config examples

### Simple virtual host

File `/etc/nginx/conf.d/hello.com.conf`:

```
server {
    listen 80;
    server_name hello.com;
    return 200 "From hello.com";
}
```

### PHP app (1)

File `/etc/nginx/conf.d/hello.com.conf`:

```
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
        fastcgi_pass api-php:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

### PHP app (2)

File `/etc/nginx/conf.d/qm.hello.com.conf`:

```
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

```
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
