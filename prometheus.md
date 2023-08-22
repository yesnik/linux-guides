# Prometheus

[Prometheus](https://github.com/prometheus/prometheus) is a systems and service monitoring system written in Go Lang. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed.

## Installation

See [docs](https://prometheus.io/docs/prometheus/latest/getting_started/)

### Install via Docker

```
mkdir data
chmod 777 data
docker run --rm -p 9090:9090 -v ./data:/prometheus prom/prometheus
```

### Install binary

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

Actual config params we can see in Prometheus Web Interface, at menu *Status - Command-Line Flags*

## Metrics

Access Prometheus server metrics from localhost:

- Open Web Browser: http://123.123.123.123:9090/metrics
- Use curl: `curl localhost:9090/metrics`

### Examples

- `go_info`
- `up`
- Filter metrics by job name: `up{job="prometheus"}`

## Exporters

[Prometheus exporters](https://prometheus.io/docs/instrumenting/exporters/) help in exporting existing metrics from third-party systems as Prometheus metrics.

### `node_exporter`

[node_exporter](https://github.com/prometheus/node_exporter) is a Prometheus exporter for hardware and OS metrics exposed by *NIX kernels, written in Go with pluggable metric collectors.

**Installation**

Current version at [download page](https://prometheus.io/download/#node_exporter)

```bash
cd /opt
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
tar xzvf node_exporter-1.6.1.linux-amd64.tar.gz
mv node_exporter-1.6.1.linux-amd64 node_exporter
cd node_exporter
./node_exporter
```

We can activate required collectors during the start of `node_exporter`. Look at all available options:

```
./node_exporter --help
```

Example Options:

- `--web.listen-address=":9100` - Addresses on which to expose metrics and web interface. Repeatable for multiple addresses.
- `--web.telemetry-path="/metrics"` - Path under which to expose metrics.

Visit URL http://123.123.12.11:9100/metrics to see all metrics of the OS.

#### Metrics Examples

- `node_disk_read_bytes_total` - bytes that were read from the disk
- `node_disk_read_time_seconds_total`

#### Metrics Format

*Prometheus metric* is just a string with metric's name, tags and value.

```
metric_name{tag1="key1", tag2="key2"} Value
# Example:
node_cpu_seconds_total{cpu="0",mode="user"} 54.15
```
We can add tag `{env="prod"}` or `{env="dev"}` to filter metrics of Test and Prod environments.

#### Create service for systemd

Add file `/etc/systemd/system/node_exporter.service`:

```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/node_exporter/node_exporter

[Install]
WantedBy=multi-user.target
```
