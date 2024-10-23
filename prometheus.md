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

### Check syntax of prometheus.yml

```bash
./promtool check config prometheus.yml
# Checking prometheus.yml
#  SUCCESS: prometheus.yml is valid prometheus config file syntax
```

## Metrics

Access Prometheus server metrics from localhost:

- Open Web Browser: http://123.123.123.123:9090/metrics
- Use curl: `curl localhost:9090/metrics`

### Metrics Examples

- `node_disk_read_bytes_total` - bytes that were read from the disk
- `node_disk_read_time_seconds_total`
- `node_disk_reads_completed_total` - disk reads count

### Metrics Format

*Prometheus metric* is just a string with metric's name, tags and value:

```
node_cpu_seconds_total{cpu="0",mode="user"} 54.15
```

Here:

- `node_cpu_seconds_total` - metric name
- `cpu="0", mode="user"` - tags. We can add tag `{env="prod"}` or `{env="dev"}` to filter metrics of Test and Prod environments
- `54.15` - metric value

### PromQL Examples

- `go_info`
- `up`
- `up{job="prometheus"}` - filter metrics by job name
- `node_disk_written_bytes_total{env="dev"}[5m]` - with tag `env="dev"` for last 5 minutes
- `node_network_receive_bytes_total` - show amount of received bytes

## Exporters

[Prometheus exporters](https://prometheus.io/docs/instrumenting/exporters/) help in exporting existing metrics from third-party systems as Prometheus metrics.

### `node_exporter`

[node_exporter](https://github.com/prometheus/node_exporter) is a Prometheus exporter for hardware and OS metrics exposed by *NIX kernels, written in Go with pluggable metric collectors.

**Installation**

Current version at [download page](https://prometheus.io/download/#node_exporter)

```bash
cd /opt
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xzvf node_exporter-1.7.0.linux-amd64.tar.gz
mv node_exporter-1.7.0.linux-amd64.tar.gz node_exporter
cd node_exporter
```

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
ExecStart=/opt/node_exporter/node_exporter \
    --collector.interrupts

[Install]
WantedBy=multi-user.target
```

We can activate required collectors during the start of `node_exporter`. Look at all available options:

```
./node_exporter --help
```

Example Options:

- `--web.listen-address=":9100` - Addresses on which to expose metrics and web interface. Repeatable for multiple addresses.
- `--web.telemetry-path="/metrics"` - Path under which to expose metrics.
- `--collector.interrupts` - Enable the interrupts collector (default: disabled)

#### Run node_exporter

```bash
service node_exporter start
```

Visit URL http://123.123.12.11:9100/metrics to see all metrics of the OS.

#### Connect Prometheus to node_exporter

Edit `prometheus.yml`:

```yml
global:
  scrape_interval:     15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'node'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']
        labels:
          env: 'dev'
```

Restart services:

```bash
service node_exporter restart
service prometheus restart
```

#### Check status of exporters

Visit web interface at http://some-domain:9090/graph and check metric `up`

Or use `promtool` utility:

```bash
./promtool query instant http://localhost:9090 up
# up{env="dev", instance="localhost:9100", job="node"} => 1 @[1700918687.859]
# up{instance="localhost:9090", job="prometheus"} => 1 @[1700918687.859]
```
Value `1` means that exporter is available for Prometheus.

### `redis-exporter`

Link: https://github.com/oliver006/redis_exporter

```bash
cd /opt
wget https://github.com/oliver006/redis_exporter/releases/download/v1.52.0/redis_exporter-v1.52.0.linux-amd64.tar.gz
tar zxfv redis_exporter-v1.52.0.linux-amd64.tar.gz
mv redis_exporter-v1.52.0.linux-amd64 redis_exporter
```

Start exporter:

```bash
./redis_exporter

# Or with config params
./redis_exporter -redis.addr redis://localhost:6379 -web.listen-address=:9121 -web.telemetry-path=/metrics
```
URL for Redis metrics: http://123.123.23.44:9121/metrics

Metrics:

- `redis_commands_total` - number of commands executed by Redis server

#### Connect Prometheus to redis-exporter

Edit `prometheus.yml`:

```
global:
  scrape_interval:     15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'redis'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9121']
```

### postgres_exporter

Link: https://github.com/prometheus-community/postgres_exporter

```bash
cd /opt
wget https://github.com/prometheus-community/postgres_exporter/releases/download/v0.13.2/postgres_exporter-0.13.2.linux-amd64.tar.gz
tar zxvf postgres_exporter-0.13.2.linux-amd64.tar.gz
mv postgres_exporter-0.13.2.linux-amd64 postgres_exporter
cd postgres_exporter
```

Create user for exporter:

```bash
su - postgres
psql

create user exporter with password '123';
alter user exporter superuser;
```

Run command with the value of environment variable `DATA_SOURCE_NAME`:

```
DATA_SOURCE_NAME="postgresql://exporter:123@localhost:5432/postgres?sslmode=disable" ./postgres_exporter
```

Access metrics: http://123.123.194.14:9187/metrics

Edit `prometheus.yml`:

```yml
scrape_configs:
  - job_name: 'postgres'
    static_configs:
    - targets: ['localhost:9187']
```

### mysqld_exporter

Link: https://github.com/prometheus/mysqld_exporter

```bash
cd /opt
wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.15.0/mysqld_exporter-0.15.0.linux-amd64.tar.gz
tar xzvf mysqld_exporter-0.15.0.linux-amd64.tar.gz
mv mysqld_exporter-0.15.0.linux-amd64 mysqld_exporter
cd mysqld_exporter
```

Create user in MySQL for exporter:

```
CREATE USER 'exporter'@'localhost' IDENTIFIED BY '123' WITH MAX_USER_CONNECTIONS 3;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
FLUSH PRIVILEGES;
```

File `/opt/mysqld_exporter/.my.cnf`:

```
[client]
user=exporter
password=123
```

File `/etc/systemd/system/mysqld_exporter.service`:

```
[Unit]
Description=Mysqld Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/mysqld_exporter/mysqld_exporter \
    --config.my-cnf /opt/mysqld_exporter/.my.cnf \
    --collect.global_status \
    --collect.info_schema.innodb_metrics \
    --collect.auto_increment.columns \
    --collect.info_schema.processlist \
    --collect.binlog_size \
    --collect.info_schema.tablestats \
    --collect.global_variables \
    --collect.info_schema.query_response_time \
    --collect.info_schema.userstats \
    --collect.info_schema.tables \
    --collect.perf_schema.tablelocks \
    --collect.perf_schema.file_events \
    --collect.perf_schema.eventswaits \
    --collect.perf_schema.indexiowaits \
    --collect.perf_schema.tableiowaits \
    --collect.slave_status \
    --web.listen-address=0.0.0.0:9111

[Install]
WantedBy=multi-user.target
```

Edit `prometheus.yml`:

```
scrape_configs:
  - job_name: 'mysql'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9111']
```

### prometheus-nginxlog-exporter

Link: https://github.com/martin-helmich/prometheus-nginxlog-exporter

Export metrics from Nginx access log files to Prometheus.

See: [releases page](https://github.com/martin-helmich/prometheus-nginxlog-exporter/releases)

```bash
cd /opt
wget https://github.com/martin-helmich/prometheus-nginxlog-exporter/releases/download/v1.11.0/prometheus-nginxlog-exporter_1.11.0_linux_amd64.deb
dpkg -i prometheus-nginxlog-exporter_1.11.0_linux_amd64.deb
```

Show status:
```bash
systemctl status prometheus-nginxlog-exporter.service
```
