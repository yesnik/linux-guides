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

## OpenSSL as CSPRING

A *cryptographically secure pseudorandom number generator* (CSPRNG) is a pseudorandom number generator (PRNG) with properties that make it suitable for use in cryptography.

```bash
openssl rand -base64 32
# Ubke5x/TfmG72ImlD+GYLtxxZBvpmkymDWhES2nbwHs=

openssl rand -hex 32
# 0afc57290634675f14be272e6c4a941c346e1a02c29f4a6ce34e935a0cf9ef4d
```

The OpenSSL command has `-base64` and `-hex` options for switching between `base64` and `hex` output.

The final argument is the number of bytes of entropy to draw.
