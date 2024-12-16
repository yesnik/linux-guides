# Prometheus Alerts

See [Awesome Prometheus Alerts](https://samber.github.io/awesome-prometheus-alerts/)

Usually we need to monitor 4 elements: CPU, Memory, HDD, Network.

Edit `/opt/prometheus/prometheus.yml`:

```yml
rule_files:
  - "/opt/prometheus/alerts.yml"
```

Add `/opt/prometheus/alerts.yml`:

```yml
groups:
- name: Alerts
  rules:
  - alert: CompactionTimeTooLong
    expr: histogram_quantile(0.95, rate(prometheus_tsdb_compaction_duration_seconds_bucket[5m])
    for: 5m
    labels:
      severity: warning
      env: dev
    annotations:
      summary: Compaction time on {{ $labels.instance }} equals {{ $value }}
      description: "Compaction time on {{ $labels.instance }} equals {{ $value }}\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

Check syntax:

```
check rules /opt/prometheus/alerts.yml
```

Restart Prometheus:

```bash
service prometheus restart
service prometheus status
```

## Alert - Free space on disk

```yml
  - alert: HostOutOfDiskSpace
    expr: (node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes < 10 and ON (instance, device, mountpoint) node_filesystem_readonly == 0
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: Host out of disk space (instance {{ $labels.instance }})
      description: "Disk is almost full (< 10% left)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

## Alert - overloaded servers

```yml
  - alert: HostHighCpuLoad
    expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100) > 80
    for: 0m
    labels:
      severity: warning
    annotations:
      summary: Host high CPU load (instance {{ $labels.instance }})
      description: "CPU load is > 80%\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

## Alert - Free memory

```yml
  - alert: HostOutOfMemory
    expr: node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100 < 10
    for: 2m
    labels:
      severity: warning
    annotations:
      summary: Host out of memory (instance {{ $labels.instance }})
      description: "Node memory is filling up (< 10% left)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

## Alert - Network

```yml
  - alert: HostUnusualNetworkThroughputIn
    expr: sum by (instance) (rate(node_network_receive_bytes_total[2m])) / 1024 / 1024 > 100
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: Host unusual network throughput in (instance {{ $labels.instance }})
      description: "Host network interfaces are probably receiving too much data (> 100 MB/s)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```
