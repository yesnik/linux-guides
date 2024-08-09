# Nginx Rewrite Module

See [docs](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html)

The directives for general‑purpose NGINX rewrite are `return`, `rewrite` and the `try_files`. 
They helps to direct requests to application servers.

Syntax:

```
rewrite <regex> <final> [flag]
```

## The `return` directive

It's better to use `return` instead of rewrite when possible. 
You enclose the return in a server or location context that specifies the URLs to be rewritten, 
and it defines the corrected (rewritten) URL for the client to use in future requests for the resource.

### Example 1. Redirect clients to a new domain name

```nginx
server {
    listen 80;
    listen 443 ssl;
    server_name www.old.com;

    return 301 $scheme://www.new.com$request_uri;
}
```

## The `rewrite` directive

### Example 1. Change URL

```nginx
server {
    listen 80;
    server_name some.com;

    location /adm/ {
        rewrite ^/adm/(.*)$ /secret/$1 permanent;
    }
}
```

Visit /adm/123?a=1, and we'll be 301 redirected to /secret/123?a=1

## The `try_files` directive

`try_files` directive is placed in a `server` or `location` block. 

As parameters, it takes a list of one or more files and directories and a final URI.

### Example 1. Default image

```nginx
location /images/ {
    try_files $uri $uri/ /images/default.gif;
}

location = /images/default.gif {
    expires 30s;
}
```
