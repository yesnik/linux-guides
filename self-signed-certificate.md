# Self Signed Certificate

1. Generate certificate `domain.crt` and private key `domain.key`:
  ```
  openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes \
    -keyout domain.key -out domain.crt -extensions san -config \
    <(echo "[req]"; 
      echo distinguished_name=req; 
      echo "[san]"; 
      echo subjectAltName=DNS:sales-test.lan.hello.com,DNS:*.sales-test.lan.hello.com,IP:10.200.8.15
      ) \
    -subj "/CN=sales-test.lan.hello.com"
  ```
  Note: This is a wildcard cert. It will help you to visit:
  - sales-test.lan.hello.com
  - some1.sales-test.lan.hello.com
  - some2.sales-test.lan.hello.com
2. Add these 2 files to nginx config:
  ```
  server {
    listen 443 ssl http2; # managed by Certbot

    ssl_certificate /etc/nginx/ssl/domain.crt;
    ssl_certificate_key /etc/nginx/ssl/domain.key;
    # ...
  }
  ```
3. On Windows: 
  - download `domain.crt`
  - Right-mouse click and select item "Install certificate" in the context menu 
  - Select importing for current user
  - **Important:** Choose *Root* storage: on the step *Ceritificate store* 
    select Place all certificates in the following storage: *Trusted Root Certification Authorities*
  - You'll see security warning. Click OK.

### Resources

- [How to import self-signed cert on Windows](https://www.barco.com/en/support/knowledge-base/3724-how-to-install-selfsigned-certificate-for-wics2100wipg1600w-to-make-the-admin-webpage-secured)
