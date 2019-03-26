# Curl

## POST query

```
curl -d "a=1&b=2" -X POST http://127.0.0.1:8000/index.php
```

This will send headers to server:

```
Host: 127.0.0.1:8000
User-Agent: curl/7.58.0
Accept: */*
Content-Length: 7
Content-Type: application/x-www-form-urlencoded
```
