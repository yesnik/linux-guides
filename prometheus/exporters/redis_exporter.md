# Redis Exporter

Link: https://github.com/oliver006/redis_exporter

## Installation

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
./redis_exporter -redis.addr redis://localhost:6379 \
    -web.listen-address=:9121 \
    -web.telemetry-path=/metrics
```

Or we can create systemd service file at `/etc/systemd/system/redis_exporter.service`:

```
[Unit]
Description=Redis Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/redis_exporter/redis_exporter \
    -redis.addr redis://localhost:6379 \
    -web.listen-address=:9121 \
    -web.telemetry-path=/metrics

[Install]
WantedBy=multi-user.target
```

Check Redis metrics:

```
curl http://localhost:9121/metrics
```

## Metrics

- `redis_commands_total` - number of commands executed by Redis server
- `redis_commands_total{cmd="config"}` - number of executed `config` commands

## Connect Prometheus to redis-exporter

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

Restart Prometheus:
```
systemctl restart prometheus
```

Check:

```bash
/promtool query instant http://localhost:9090 up
# ..
# up{instance="localhost:9121", job="redis"} => 1 @[1617801868.196]
```
