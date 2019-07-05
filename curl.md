# Curl

## POST query

### application/x-www-form-urlencoded

```bash
curl -d 'a=1&b=2' -X POST http://127.0.0.1:8000/index.php
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
curl -d '{"a":1, "b":"wow"}' -H "Content-Type: application/json" -X POST http://127.0.0.1:8000/index.php
```

This will send headers to server:

```bash
Host: 127.0.0.1:8000
User-Agent: curl/7.58.0
Accept: */*
Content-Type: application/json
Content-Length: 20
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
curl -X DELETE http://localhost:3000/
```

## HEAD query

```bash
curl -X PUT -d a=1 http://localhost:3000/
```
