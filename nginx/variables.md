
## Nginx Variables

See: [allowed variables](https://nginx.org/en/docs/varindex.html)

### Variables from Headers

Also we can use values of all headers that client has sent to us. Header `X-My-VaR: 123` will be available in the variable `$http_x_my_var`.

```nginx
server {
    listen 80;
    root /var/www/html;
    
    server_name _;
    
    location / {
        add_header X-Remote-addr $remote_addr;
        add_header X-My-New-Request $http_x_my_request;
        return 200;
    }
}
```
Test:

```
curl -H "X-My-Request: hey" -v http://example.com/
# < X-Remote-addr: 122.22.33.44
# < X-My-New-Request: hey
```
As we can see nginx copied value from `X-My-Request` to `X-My-New-Request` header.

### Directive `set`

We can define custom variables and assign values to them:

```nginx
server {
    listen 80;
    
    if ($host ~* (.*)\.example\.com) {
        set $sub_domain $1;
    }
    
    location / {
        add_header X-Sub-Domain $sub_domain;
        return 200;
    }
}
```
Test:
```
curl -v http://some.example.com/
```
Server will return header `X-Sub-Domain: some`. 

### Directive `map`

```nginx
map $remote_addr $remote_access {
    default block;

    10.0.0.1 allow;
    10.0.0.2 allow;
}

server {
    if ($remote_access = block) {
        return 403;
    }
    
    location / {
        return 200;
    }
}
```

### Directive `if`

```nginx
server {
    listen 80;
    
    if ($http_user_agent ~* "curl.*") {
        return 500 'Oops curl detected';
    }
    
    location / {
        return 200;
    }
}
```

Test:

```
curl -v http://example.com/
```

It returns 500 status code and body "Oops curl detected".

We can bypass this check:

```
curl -H "User-agent: Chrome" -v http://example.com/
```
