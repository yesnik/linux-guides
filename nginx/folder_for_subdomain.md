# Folder for subdomain

If we want to open each subdomain in a different folder:

- https://feature1.mysite.com - at `/var/www/feature1`
- https://feature2.mysite.com - at `/var/www/feature2`

Edit `/etc/nginx/conf.d/site.conf`:

```nginx
server {
    listen 80;
    server_name ~^(?<branch>.+)\.mysite.com$;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    
    set $host_path "/var/www";
    server_name ~^(?<branch>.+)\.mysite.com$;
    root $host_path/html/$branch;

    index index.html index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php {
        fastcgi_index   index.php;
        fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
        include fastcgi_params;
    }
}
```

