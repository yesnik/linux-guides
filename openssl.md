# Open SSL

[OpenSSL](https://www.openssl.org/) is a toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols. 
It is also a general-purpose cryptography library.

## Generate keys

- Private key:
  ```
  openssl genrsa -out private.pem 3072
  ```
- Public key for private key:
  ```
  openssl rsa -in private.pem -pubout -out public.pem
  ```
