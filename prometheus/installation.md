# Prometheus

[Prometheus](https://github.com/prometheus/prometheus) is a systems and service monitoring system written in Go Lang. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed.

## Installation

See [official documentation](https://prometheus.io/docs/prometheus/latest/getting_started/)

### Install via Docker

```
mkdir data
chmod 777 data
docker run --rm -p 9090:9090 -v ./data:/prometheus prom/prometheus
```

### Install binary

Latest binary: https://prometheus.io/download/

```bash
cd /opt/
wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
tar xzvf prometheus-2.48.0.linux-amd64.tar.gz
mv prometheus-2.48.0.linux-amd64 prometheus
cd prometheus
# Create folder for Time Series Database
mkdir data

# Create user "prometheus" and service for systemd
useradd -s /sbin/nologin prometheus
chown -R prometheus:prometheus /opt/prometheus
```

Add file `/etc/systemd/system/prometheus.service`:

```
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
    --web.console.libraries=/opt/prometheus/console_libraries

[Install]
WantedBy=default.target
```

Start prometheus:

```bash
systemctl daemon-reload
systemctl enable prometheus
systemctl start prometheus
systemctl status prometheus
```
Visit http://your-site:9090/ to see Prometheus Admin

## Folders

- `console_libraries` - contains libs for HTML templates
- `consoles` - contains HTML templates for web pages
- `prometheus` - Prometheus binary file (size ~120 Mb)
- `prometheus.yml` - config file
- `promtool` - util (size ~120 Mb) for check config, working with TSDB (time series DB), receiving metrics
