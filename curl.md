# Curl

**Important:** If you have several query params in URL, wrap URL in quotes. Otherwise server will get only first param:

```bash
# BAD
curl http://127.0.0.1:8000/?a=1&b=2

# GOOD
curl "http://127.0.0.1:8000/?a=1&b=2"
```

## Params

- **-v** Show headers of request and response:
  ```
  curl -v http://127.0.0.1:8000
  ```
- **-D** Dump response headers:
  ```
  curl -D - http://google.com
  ```
- **-I** Show only headers of response, without body:
  ```
  curl -I http://127.0.0.1:8000
  ```

## POST query

### application/x-www-form-urlencoded

```bash
curl -X POST -d 'a=1&b=2' http://127.0.0.1:8000/index.php
```

This will send headers to server:

```bash
Host: 127.0.0.1:8000
User-Agent: curl/7.58.0
Accept: */*
Content-Type: application/x-www-form-urlencoded
Content-Length: 7
```

### application/json

```bash
curl -X POST -H "Content-Type: application/json" -d '{"a":1, "b":"wow"}' http://127.0.0.1:8000/index.php
```

This will send headers to server:

```bash
Host: 127.0.0.1:8000
User-Agent: curl/7.58.0
Accept: */*
Content-Type: application/json
Content-Length: 20
```

**Add Cookie header with session ID**

```bash
curl -X POST -H "Content-Type: application/json" -H "Cookie: PHPSESSID=f2je0;" -d '{"postId": 123}' "https://www.site.com/vote/22"
```

In PHP you can get passed JSON values like this:

```php
<?php
print_r( json_decode( file_get_contents('php://input'), true ) );

/* Output:
['a' => 1, 'b' => 'wow']
*/
?>
```

## DELETE query

```bash
curl -X DELETE http://127.0.0.1:3000/users/3
```

## PUT query

```bash
curl -X PUT -d 'a=1&b=2' "http://127.0.0.1:8000/users/2"
```

## Curl Proxy

```bash
curl -vvv -x "http://user:pass@178.17.0.221:80" "http://getcomposer.org/versions"
```

Another way to use proxy with curl is to set the environment variables `http_proxy` and `https_proxy`:

```bash
export http_proxy="http://user:pass@127.0.0.1:1234"
export https_proxy="http://user:pass@127.0.0.1:1234"
```
After running these two commands, run curl normally:

```bash
curl "http://httpbin.org/ip"
```
