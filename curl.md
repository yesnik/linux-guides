# Curl

## POST query

### application/x-www-form-urlencoded

```
curl -d 'a=1&b=2' -X POST http://127.0.0.1:8000/index.php
```

This will send headers to server:

```
Host: 127.0.0.1:8000
User-Agent: curl/7.58.0
Accept: */*
Content-Type: application/x-www-form-urlencoded
Content-Length: 7
```

### application/json

```
curl -d '{"a":1, "b":"wow"}' -H "Content-Type: application/json" -X POST http://127.0.0.1:8000/index.php
```

This will send headers to server:

```
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
