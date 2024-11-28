# Pushgateway

[Pushgateway](https://github.com/prometheus/pushgateway) is a push acceptor for ephemeral and batch jobs.

It's recommended to use it for cron jobs or other short living processes (see [details](https://prometheus.io/docs/practices/pushing/)).

## Installation

### Binary

Actual release: https://github.com/prometheus/pushgateway/releases

```bash
cd /opt
wget https://github.com/prometheus/pushgateway/releases/download/v1.10.0/pushgateway-1.10.0.linux-amd64.tar.gz
tar -xzvf pushgateway-1.10.0.linux-amd64.tar.gz
cd pushgateway-1.10.0.linux-amd64
./pushgateway & disown
```

### Docker

```bash
docker run -d -p 9091:9091 prom/pushgateway
```

## Web interface

http://<ip-address>:9091/

## Send data to pushgateway

### Define tags in the URL

These metrics will belong to different groups.

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

Send metrics:

```bash
cat <<EOF | curl --data-binary @- http://localhost:9091/metrics/job/cron
# TYPE cron_processed_sum gauge
cron_processed_sum 65000
# TYPE cron_processed_events gauge
# HELP cron_processed_events Processed Events Counter.
cron_processed_events 653
EOF
```

### Define tags in the metric body

These metrics will belong to the same group.

```bash
cat <<EOF | curl --data-binary @- http://localhost:9091/metrics/job/cron
cron_users{type="good"} 560
cron_users{type="bad"} 24
EOF
```

## Send info to pushgateway

```bash
echo 'cron_processed_events 25' | curl --data-binary @- http://127.0.0.1:9091/metrics/job/cron
```

Metrics are available at address: http://127.0.0.1:9091/metrics

## Delete metrics' group

Add metric to the `instance/db3` group:
```
echo 'myapp_processed{type="payments"} 56' | curl --data-binary @- http://localhost:9091/metrics/job/myapp/instance/db3
```

Delete `instance/db3` group of metrics:

```bash
curl -X DELETE http://localhost:9091/metrics/job/myapp/instance/db3
```

## Metrics

```bash
curl http://localhost:9091/metrics
# ...
# HELP push_failure_time_seconds Last Unix time when changing this group in the Pushgateway failed.
# TYPE push_failure_time_seconds gauge
push_failure_time_seconds{instance="",job="cron"} 0
# HELP push_time_seconds Last Unix time when changing this group in the Pushgateway succeeded.
# TYPE push_time_seconds gauge
push_time_seconds{instance="",job="cron"} 1.5408582544334151e+09
```

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
