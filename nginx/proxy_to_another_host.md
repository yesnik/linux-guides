# Nginx. Proxy to another host

```
server {
    listen 80;
    server_name mysite.com;

    location /hello {
        proxy_pass http://another.com/path;
    }
}
```

- Visit http://mysite.com/hello
- You'll see the contents of page http://another.com/path
- Browser's address bar still shows http://mysite.com/hello
