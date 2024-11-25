# Node Exporter

[node_exporter](https://github.com/prometheus/node_exporter) is a Prometheus exporter for hardware and OS metrics exposed by *NIX kernels, written in Go with pluggable metric collectors.

**Installation**

Current version at [download page](https://prometheus.io/download/#node_exporter)

```bash
cd /opt
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
tar xzvf node_exporter-1.8.2.linux-amd64.tar.gz
mv node_exporter-1.8.2.linux-amd64.tar.gz node_exporter
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

Visit URL http://123.123.12.11:9100/metrics to see all (~ 500) metrics of the OS.

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
