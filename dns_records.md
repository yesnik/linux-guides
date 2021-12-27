# DNS Records

## Commands

- `host google.ru` - show IP addressed for domain name

## Check DNS server

This command will return `A` record for the domain `mysite.com` using DNS server `ns1.hellovds.com`:

```
nslookup -q=A mysite.com ns1.hellovds.com
```
