# Nginx Logs

## Log format

### Default format

By default nginx has this log format at `/etc/nginx/nginx.conf`:

```nginx
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
}
```

### JSON format

```nginx
http {
    log_format main escape=json '{"remote_addr":"$remote_addr","remote_user":"$remote_user","time":"[$time_local]","request":"$request",'
     '"status":"$status","body_bytes_sent":"$body_bytes_sent","http_referer":"$http_referer",'
     '"http_user_agent":"$http_user_agent","http_x_forwarded_for":"$http_x_forwarded_for"}';

    access_log  /var/log/nginx/access.log  main;
```

Example of the log message:

```json
{"remote_addr":"172.26.0.1","remote_user":"","time":"[08/Apr/2024:12:17:58 +0000]","request":"GET / HTTP/1.1","status":"304","body_bytes_sent":"0","http_referer":"","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36","http_x_forwarded_for":""}
```
