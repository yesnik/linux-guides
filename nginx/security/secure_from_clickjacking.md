# Secure Nginx from Clickjacking

The `X-Frame-Options` in HTTP response header can be used to indicate whether or not a browser should be allowed to open a page in frame or iframe.

This will prevent site content embedded into other sites.

There are three settings for `X-Frame-Options`:

- `SAMEORIGIN`: This setting will allow the page to be displayed in a frame on the same origin as the page itself.
- `DENY`: This setting will prevent a page displaying in a frame or iframe.
- `ALLOW-FROM URI`: This setting will allow a page to be displayed only on the specified origin.

```
server {
    # ...
    add_header X-Frame-Options "SAMEORIGIN";
}
```
