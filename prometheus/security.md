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

## Node exporter behind nginx

Generate a key for node_exporter:

```bash
cd /opt/ssl
openssl genrsa -out node_exporter.key 4096
```
```bash
cat > req.conf << EOF
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no
[req_distinguished_name]
C = RU
ST = Moscow
L = Moscow
O = ExampleCorp
OU = IT
CN = node_exporter.example.com
[v3_req]
keyUsage = keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names
[alt_names]
DNS.1 = node_exporter.example.com
EOF
```

Create CSR:

```bash
openssl req -new -key node_exporter.key -out node_exporter.csr -config req.conf -extensions 'v3_req'
```

Generate certificate:

```bash
openssl x509 -req -days 365 -in node_exporter.csr -CA ca.crt -CAkey ca.key -set_serial 1 -extensions 'v3_req' -extfile req.conf -out node_exporter.crt
```
Here we used `req.conf` where we defined SAN names for our host. Without them Prometheus will return similar error:

```
Get "https://127.0.0.1:9101/metrics": x509: certificate relies on legacy Common Name field, use SANs or temporarily enable Common Name matching with GODEBUG=x509ignoreCN=0
```

Install nginx:
```
apt install nginx
```

Edit `/etc/sites-enabled/node_exporter.conf`:

```nginx
server {
  listen 9101 ssl;

  ssl_certificate /opt/ssl/node_exporter.crt;
  ssl_certificate_key /opt/ssl/node_exporter.key;

  server_name node_exporter.example.com;

  location / {
    proxy_pass http://127.0.0.1:9100/;
    auth_basic "Restricted Area";
    auth_basic_user_file .htpasswd;
  }
}
```

Here we started nginx on port 9101 and secured it with basic auth.

Create file `.htpasswd` for nginx:

```bash
htpasswd -cbBC 10 /etc/nginx/.htpasswd "kenny" 123
```

Restart nginx:
```
service nginx restart
```

Install node exporter:

```bash
cd /opt
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
tar xzvf node_exporter-1.8.2.linux-amd64.tar.gz
mv node_exporter-1.8.2.linux-amd64 node_exporter
cd node_exporter
```

Edit file `/etc/systemd/system/node_exporter.service`:

```systemd
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/node_exporter/node_exporter \
    --web.listen-address="127.0.0.1:9100" \
    --collector.interrupts

[Install]
WantedBy=multi-user.target
```

Here we starts node exporter on the *loopback* interface. Only nginx can connect to it.

Test connection: 

```bash
curl -D - -u kenny:123 --cacert /opt/ssl/ca.crt --resolve node_exporter.example.com:9101:127.0.0.1 https://node_exporter.example.com:9101
```

Add node exporter to Prometheus. Edit `/opt/prometheus/prometheus.yml`:

```yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: node-exporter
    static_configs:
    - targets: ['127.0.0.1:9101']
    scheme: https
    tls_config:
      ca_file: /opt/ssl/ca.crt
      server_name: node_exporter.example.com
    basic_auth:
      username: kenny
      password: 123
```

Restart Prometheus:

```bash
service prometheus restart
service prometheus status
```
