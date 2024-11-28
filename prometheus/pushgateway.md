# Pushgateway

[Pushgateway](https://github.com/prometheus/pushgateway) is a push acceptor for ephemeral and batch jobs.

It's recommended to use it for cron jobs or other short living processes.

## Installation

Actual release: https://github.com/prometheus/pushgateway/releases

```bash
cd /opt
wget https://github.com/prometheus/pushgateway/releases/download/v1.10.0/pushgateway-1.10.0.linux-amd64.tar.gz
tar -xzvf pushgateway-1.10.0.linux-amd64.tar.gz
cd pushgateway-1.10.0.linux-amd64
./pushgateway & disown
```

## Web interface

http://<ip-address>:9091/

## Send data to pushgateway

File `type.txt`:

```bash
# TYPE cron_processed_items Counter
cron_processed_items 100
```

Create item with tag `job = "cron"`:

```bash
curl --data-binary @type.txt http://127.0.0.1:9091/metrics/job/cron
```

Create item with tag `job = "cron"` and `instance="app1"`:

```bash
curl --data-binary @type.txt http://127.0.0.1:9091/metrics/job/cron/instance/app1
```

## Send info to pushgateway

```bash
echo 'cron_processed_events 25' | curl --data-binary @- http://127.0.0.1:9091/metrics/job/cron
```

Metrics are available at address: http://127.0.0.1:9091/metrics

## Add to Prometheus

Edit `prometheus.yml`:

```
  - job_name: 'pushgateway'
  honor_labels: true
  static_configs:
  - targets: ['localhost:9091']
```

Option `honor_labels: true` orders Prometheus not to replace tags: 

- `job` with `pushgateway`
- `instance` with `localhost:9091`

It's because clients define this tags themselves when send messages to pushgateway.
