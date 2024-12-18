# Prometheus Federation

Federation allows a Prometheus server to scrape selected time series from another Prometheus server.

## Config child Prometheus

File on child Prometheus server `/opt/prometheus/prometheus.yml`:

```yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s
  external_labels:
    datacenter: dc_1

rule_files:
  - "rules.yml"

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
    - targets: ['localhost:9100']
```

File on child Prometheus server `/opt/prometheus/rules.yml`:

```yml
groups:
  - name: global_data_center
    interval: 5s
    rules:
    - record: job:node_memory_MemTotal_bytes:sum
      expr: sum without(instance)(node_memory_MemTotal_bytes{job="node"})
```

Here we created group `global_data_center`. Every 5s query will be executed and result will be saved in the metric `job:node_memory_MemTotal:sum`.

See [recording rules](https://prometheus.io/docs/practices/rules/#naming-and-aggregation)

Restart Prometheus on the child server:

```
service prometheus restart
service prometheus status
```

## Config parent Prometheus

Edit `/opt/prometheus/prometheus.yml`:

```yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: federation
    honor_labels: true
    metrics_path: /federate
    params:
      match[]:
        - '{__name__=~"^job:.*"}'
    static_configs:
      - targets:
        - <IP address of child Prometheus>:9090
```

Param `metrics_path` tells Prometheus to use this URL for metrics scraping.

Restart Prometheus on the parent server:

```
service prometheus restart
service prometheus status
```

Let's try to get metric on our parent Prometheus server:

```bash
./promtool query instant http://localhost:9090/ 'job:node_memory_MemTotal_bytes:sum'
# job:node_memory_MemTotal_bytes:sum{datacenter="dc_1", job="node"} => 1053201113 @[1623744499.26]
```
