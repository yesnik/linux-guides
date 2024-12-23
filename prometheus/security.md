# Prometheus Security

## Create a root CA certificate

### Create the root key

This creates an encrypted key.

```bash
mkdir -p /opt/ssl
cd /opt/ssl
openssl genrsa -out ca.key 4096
```

### Create a Root Certificate and self-sign it

Use the following command to generate the Root Certificate.

```bash
openssl req -new -x509 -days 720 -key ca.key -out ca.crt
```
We'll use this to sign our server certificate.

## Generate server key and certs

Generate server key:

```bash
openssl genrsa -out server.key 4096
```

The CSR is a public key that is given to a CA when requesting a certificate. The CA issues the certificate for this specific request.

Generate CSR for the key. 

```bash
openssl req -new -key server.key -out server.csr
```

Generate server cert:

```bash
openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 1 -out server.crt
```

So we got 2 files:

- server.crt
- server.key

## Prometheus config

Install tool to generate password:

```bash
apt install apache2-utils
```
Generate hash for user / pass - `kenny` / `123`:

```bash
htpasswd -bnBC 10 "kenny" 123
# kenny:$2y$10$1vt/2r7WhBbnkMejZwqTI.YtR5AUlPTIpH13RSbgUBRK6.UNUtXhK
```

Edit `/opt/prometheus/web.config`:

```yml
tls_server_config:
  cert_file: /opt/ssl/server.crt
  key_file: /opt/ssl/server.key

http_server_config:
  http2: true

basic_auth_users:
  kenny:$2y$10$1vt/2r7WhBbnkMejZwqTI.YtR5AUlPTIpH13RSbgUBRK6.UNUtXhK
```

Edit `/etc/systemd/system/prometheus.service`:

```systemd
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
ExecStart=/opt/prometheus/prometheus \
    --config.file=/opt/prometheus/prometheus.yml \
    --storage.tsdb.path=/opt/prometheus/data \
    --web.console.templates=/opt/prometheus/consoles \
    --web.console.libraries=/opt/prometheus/console_libraries \
    --web.config.file=/opt/prometheus/web.config

[Install]
WantedBy=default.target
```

Restart Prometheus:

```bash
service prometheus restart
service prometheus status
```

Let's make request to Prometheus:

```bash
curl -D - -u kenny:123 --cacert /opt/ssl/ca.crt --resolve example.org:9090:127.0.0.1 https://example.org:9090
```
