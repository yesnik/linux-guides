# Grafana Alerting

- [Official documentation](https://grafana.com/docs/grafana/latest/alerting/)
- [Collection of Awesome Prometheus Alerts](https://samber.github.io/awesome-prometheus-alerts/rules.html)

Grafana Alerting system helps us to watch for specific events or circumstances in our incoming metrics data or log entries.

We can set up alerts in Prometheus or Grafana - it doesn't matter which.

**Note**: Alerts should not bother you. Otherwise, you will start to ignore them.

## Config

File `/opt/prometheus/prometheus.yml`:

```yml
global:
    # Scrape metrics from exporters every 15s
    scrape_interval: 15s
  
    # Evaluate rules every 15s
    evaluation_interval: 15s

# Load rules once and periodically evaluate them according to the global "evaluation_interval"
rule_files:
    - alert_rules.yml
```

File `/opt/prometheus/alert_rules.yml`:

```yml
groups:
- name: instances
  interval: 10s
  rules:
  - alert: InstanceDown
    expr: up == 0
    for: 5m
    labels:
      severity: critical
      labelscount: "{{ len $labels }}"
    annotations:
      summary: "Instance {{ $labels.instance }} down"
      description: "{{ $labels.instance }} of job {{ $labels.job }} has been down for 5 minutes."
```

Here,

- `name` - custom group name for all alerts in this section. This name should be unique in the file.
- `expr` - PromQL query. If the query returns a result, an alert message will be generated.
- `for` - wait for this period of time to generate an alert.
- `labels` - add these labels to an alert message
- `annotations` - add these annotations to an alert message

In labels, annotations we can use [Go Lang templates](https://pkg.go.dev/text/template).

## Interface

We can see our alerting rules in the web interface: http://site.com:9090/alerts
