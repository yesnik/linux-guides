# Prometheus

[Prometheus](https://github.com/prometheus/prometheus) is a systems and service monitoring system written on Go Lang. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed.

## Installation

See [docs](https://prometheus.io/docs/prometheus/latest/getting_started/)

```bash
cd /opt/
wget https://github.com/prometheus/prometheus/releases/download/v2.46.0/prometheus-2.46.0.linux-amd64.tar.gz
tar xzvf prometheus-2.46.0.linux-amd64.tar.gz
mv prometheus-2.46.0.linux-amd64 prometheus
cd prometheus
# Create folder for Time Series Database
mkdir data

# Create user "prometheus" and service for systemd
useradd -s /sbin/nologin -d /opt/prometheus prometheus
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
- `prometheus` - Prometheus binary file, size: 118 Mb
- `prometheus.yml` - config file
- `promtool` - util for check config, working with TSDB (time series DB), receiving metrics

## Config params

We can run `./prometheus` binary with different params:

```
./prometheus --help
```

For example:

- `--config.file="prometheus.yml"` - path to config file
- `--web.listen-address="0.0.0.0:9090"` - address and port for internal web server
- `--web.enable-admin-api` - enable admin API from web interface
- `--web.console.templates="consoles"` - path to html templates
- `--web.console.libraries="console_libraries"` - path to libs for html templates
- `--web.page-title` - web page title
- `--web.cors.origin=".*"` - CORS config for web interface
- `--storage.tsdb.path="data/"` - path for storing time series database data
- `--storage.tsdb.retention.time` - time for storing metrics. Default is 15 days. Expired data will be deleted
- `--storage.tsdb.retention.size` - max size of TSDB. After reaching this value Prometheus will delete old data
- `--query.timeout=4m` - max execution time of one query
- `--enable-feature` - enable [features](https://prometheus.io/docs/prometheus/latest/feature_flags/)
- `--log.level` - logging level
