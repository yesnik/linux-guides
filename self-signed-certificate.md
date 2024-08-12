# Self Signed Certificate

## Wildcard certificate

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

## Create a Self-Signed Certificate

1. Create a private key. A private key helps to enable encryption, and is the most important component of our certificate. Let’s create a password-protected, 2048-bit RSA private key (`domain.key`):
  ```
  openssl genrsa -des3 -out domain.key 2048
  ```
   If we want our private key unencrypted, we can remove the `-des3` option from the command.
2. If we want our certificate signed, we need a *certificate signing request (CSR)*. 
  The CSR includes the public key and some additional information (such as organization and country). Let’s create a CSR (`domain.csr`) from our existing private key:
  ```
  openssl req -key domain.key -new -out domain.csr
  ```
  An important field is "Common Name" which should be the exact Fully Qualified Domain Name (FQDN) of our domain.
3. Creating a Self-Signed Certificate. A self-signed certificate is a certificate that's signed with its own private key. 
  It can be used to encrypt data just as well as CA-signed certificates, but our users will be shown a warning that says the certificate isn't trusted.
  Let's create a self-signed certificate (`domain.crt`) with our existing private key and CSR:
  ```
  openssl x509 -signkey domain.key -in domain.csr -req -days 365 -out domain.crt
  ```

## Creating a CA-Signed Certificate With Our Own CA

We can be our own certificate authority (CA) by creating a self-signed root CA certificate, and then installing it as a trusted certificate in the local browser.

1. Create a Self-Signed Root CA. Let's create a private key (`rootCA.key`) and a self-signed root CA certificate (`rootCA.crt`) from the command line:
  ```
  openssl req -x509 -sha256 -days 1825 -newkey rsa:2048 -keyout rootCA.key -out rootCA.crt
  ```
2. Sign Our CSR With Root CA. We can sign our CSR (`domain.csr`) with the root CA certificate and its private key:
  ```
  openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in domain.csr -out domain.crt -days 365 -CAcreateserial
  ```
  As a result, the CA-signed certificate will be in the `domain.crt` file. This would result in a working certificate, but browsers would still flag them. 
  This happens because of the changes to the X.509 certificates and the addition of the SAN extension.
3. To align with SAN extension standards, we need to create a configuration text file (`domain.ext`) with the following content:
  ```
  authorityKeyIdentifier=keyid,issuer
  basicConstraints=CA:FALSE
  subjectAltName = @alt_names
  [alt_names]
  DNS.1 = mysite.com
  ```
  The "DNS.1" field should be the domain of our website. 
  
  Then, we can slightly modify our previous command and add the information about the extension file:
  ```
  openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in domain.csr -out domain.crt -days 365 -CAcreateserial -extfile domain.ext
  ```
  Now, our certificate meets all the *SAN requirements* and works correctly. 
  This process requires an additional step, and openssl doesn't provide a prompt for this information, 
  so we must create a separate extension file. 
  However, SAN makes the certificates more secure. 
  Also, it allows the definition of several domains or IP addresses and *we can use a single certificate across multiple domains*.

## View Certificates

We can use the openssl command to view the contents of our certificate in plain text:

```
openssl x509 -text -noout -in domain.crt
```

## Convert Certificate Formats

Our certificate (`domain.crt`) is an X.509 certificate that's *ASCII PEM-encoded*. We can use OpenSSL to convert it to other formats for multi-purpose use.

### Convert PEM to DER

The DER format is usually used with Java. Let’s convert our PEM-encoded certificate to a DER-encoded certificate:

```
openssl x509 -in domain.crt -outform der -out domain.der
```

### Convert PEM to PKCS12

PKCS12 files, also known as PFX files, are usually used for importing and exporting certificate chains in Microsoft IIS.
We’ll use the following command to take our private key and certificate, and then combine them into a PKCS12 file:

```
openssl pkcs12 -inkey domain.key -in domain.crt -export -out domain.pfx
```

## Resources

- [How to import self-signed cert on Windows](https://www.barco.com/en/support/knowledge-base/3724-how-to-install-selfsigned-certificate-for-wics2100wipg1600w-to-make-the-admin-webpage-secured)
- [Creating a Self-Signed Certificate With OpenSSL](https://www.baeldung.com/openssl-self-signed-cert)
