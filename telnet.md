# Telnet

We use Linux for sending messages to remote server via `telnet`.

### HTTP/1.0

```bash
telnet google.com 80
GET /index.html HTTP/1.0
```

In HTTP 1.0 remote host closes connection.

### HTTP/1.1

`Host` header is required for the request in HTTP v.1.1

```bash
telnet google.com 80
GET /index.html HTTP/1.1
Host: google.com
```

In HTTP 1.1 remote host *doesn't* close connection.
