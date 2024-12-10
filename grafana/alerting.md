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
    annotations:
      summary: "Instance {{ $labels.instance }} down"
      description: "{{ $labels.instance }} of job {{ $labels.job }} has been down for 5 minutes."
```

Here,

- `name` - custom group name for all alerts in this section. This name should be unique in the file.
- `interval` - this param redefines global `evaluation_interval`
- `expr` - PromQL query. If the query returns a result, an alert message will be generated.
- `for` - wait for this period of time to generate an alert.
- `labels` - add these labels to an alert message. They helps Alertmanager to route a message to different notification channels, groups.
- `annotations` - add these annotations to an alert message

In labels, annotations we can use [Go Lang templates](https://pkg.go.dev/text/template).

## Interface

We can see our alerting rules in the web interface: http://site.com:9090/alerts

## Alert Flow

- When all is OK our alert is in the `inactive` state.
- Prometheus runs PromQL expression from `expr` periodically. Period is defined in the `interval` param.
- Alert status becomes `pending` if `expr` returns a value for a period of time defined in the `for` param.
- If `expr` continues to return a value, the alert transitions to the `firing` state and is sent to Alertmanager.
- Prometheus will send active alert to Alertmanager every 30s.

We can see alerts by executing PromQL: `"ALERTS{}"`

```
ALERTS{alertname="InstanceDown", alertstate="firing", instance="localhost:8080", job="app", labelscount="3", severity="critical"} => 1 @[1722758792.223]
```

## Tests for alerts

File `alert_rules_test.yml`:

```yml
rule_files:
    - alert_rules.yml

evaluation_interval: 10s

tests:
    - interval: 15s
      input_series: # Test metric
          - series: 'up{job="prometheus", instance="localhost:9090"}' # Name and tags of test metric
            values: '0 0 0 0 0 0 0 0 0 0 0 0 0 0 0' # Values of test metric that will be collected every "interval".
                                                    # In our example "interval 15 sec",
                                                    # first 0 - will be collected in the beginning.
                                                    # second 0 - will be collected in 15s, 
                                                    # third 0 - will be collected in 30s etc.

      # Test cases
      alert_rule_test:
          # Time from the moment of running a test - 
          # during this time tests will collect all metric's values
          # (every 15s, also check rules every 10s)
          - eval_time: 5m
            # Name of tested alert
            alertname: InstanceDown
            # Expected output
            exp_alerts:
                - exp_labels:
                      severity: critical
                      instance: localhost:9090
                      job: prometheus
                  exp_annotations:
                      summary: "Instance localhost:9090 down"
                      description: "localhost:9090 of job prometheus has been down for 1 minute."
```

Run tests:

```bash
/opt/prometheus/promtool test rules alert_rules_test.yml
```
