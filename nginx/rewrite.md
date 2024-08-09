# Nginx Rewrite Module

See [docs](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html)

The directives for general‑purpose NGINX rewrite are `return`, `rewrite` and the `try_files`. 
They helps to direct requests to application servers.

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

