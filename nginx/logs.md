# Nginx Logs

See docs: [ngx_http_log_module](https://nginx.org/en/docs/http/ngx_http_log_module.html)

## Directives

Syntax:	

```nginx
access_log path [format [buffer=size] [gzip[=level]] [flush=time] [if=condition]];
access_log off;
```

- `buffer` - logs will be stored in RAM and will be saved on disk when buffer is filled
- `flush` - save logs from RAM to disk periodically
- `gzip` - gzip buffer before saving it to the disk
- `if` - if value 0 or '' data won't be written in the log. Example - log request if header `X-Log-Request` exists:
    ```nginx
    access_log /var/log/nginx/access.log combined if=$http_x_log_request
    ```

## Log format

### Default format

By default nginx has this log format at `/etc/nginx/nginx.conf`:

```nginx
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log main;
}
```

### JSON format

```nginx
http {
    log_format main escape=json '{"remote_addr":"$remote_addr","remote_user":"$remote_user","time":"[$time_local]","request":"$request",'
     '"status":"$status","body_bytes_sent":"$body_bytes_sent","http_referer":"$http_referer",'
     '"http_user_agent":"$http_user_agent","http_x_forwarded_for":"$http_x_forwarded_for"}';

    access_log  /var/log/nginx/access.log main;
}
```

Example of the log message:

```json
{"remote_addr":"172.26.0.1","remote_user":"","time":"[08/Apr/2024:12:17:58 +0000]","request":"GET / HTTP/1.1","status":"304","body_bytes_sent":"0","http_referer":"","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36","http_x_forwarded_for":""}
```

### Format time

```nginx
http {
    log_format time '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for" '
                    '$upstream_response_time $request_time';

    access_log /var/log/nginx/access.log time;
}
```
- `$upstream_response_time` - upstream response time
- `$request_time` - could be high if a client has slow internet
