# Prometheus Alertmanager

[Alerting](https://prometheus.io/docs/alerting/latest/overview/) with Prometheus is separated into two parts:

1. Alerting rules in Prometheus servers send alerts to an Alertmanager. 
2. The Alertmanager then manages those alerts, including silencing, inhibition, aggregation and sending out notifications 
via methods such as email, on-call notification systems, and chat platforms.

## Installation

Visit [download page](https://prometheus.io/download/#alertmanager)

```
cd /opt
wget https://github.com/prometheus/alertmanager/releases/download/v0.28.0-rc.0/alertmanager-0.28.0-rc.0.linux-amd64.tar.gz
tar -xzvf alertmanager-0.28.0-rc.0.linux-amd64.tar.gz
mv alertmanager-0.28.0-rc.0.linux-amd64 alertmanager
```

Create systemd - `/etc/systemd/system/alertmanager.service`

```
[Unit]
Description=Prometheus Alertmanager
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/alertmanager/alertmanager \
    --config.file=/opt/alertmanager/alertmanager.yml

[Install]
WantedBy=multi-user.target
```

Start and check status:

```bash
service alertmanager start
service alertmanager status
```

## Condig

File `/opt/alertmanager/alertmanager.yml`:

```yml
route:
  receiver: 'default-receiver'
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  group_by: [cluster, alertname]
  # All alerts that do not match the following child routes
  # will remain at the root node and be dispatched to 'default-receiver'.
  routes:
  - reciever: 'slack'
    group_wait: 0s
    matchers:
    - severity="critical"
  # All alerts with service=mysql or service=cassandra
  # are dispatched to the database pager.
  - receiver: 'database-pager'
    group_wait: 10s
    matchers:
    - service=~"mysql|cassandra"
  # All alerts with the team=frontend label match this sub-route.
  # They are grouped by product and environment rather than cluster
  # and alertname.
  - receiver: 'frontend-pager'
    group_by: [product, environment]
    matchers:
    - team="frontend"

  # All alerts with the service=inhouse-service label match this sub-route.
  # the route will be muted during offhours and holidays time intervals.
  # even if it matches, it will continue to the next sub-route
  - receiver: 'dev-pager'
    matchers:
      - service="inhouse-service"
    mute_time_intervals:
      - offhours
      - holidays
    continue: true

    # All alerts with the service=inhouse-service label match this sub-route
    # the route will be active only during offhours and holidays time intervals.
  - receiver: 'on-call-pager'
    matchers:
      - service="inhouse-service"
    active_time_intervals:
      - offhours
      - holidays
receivers:
  - name: 'Mail Alert'
    email_configs:
      - smarthost: 'smtp.rambler.ru:465'
        auth_username: '<username>'
        auth_password: "<password>"
        from: '<email from>'
        to: '<email to>'
        headers:
          subject: 'Prometheus Email Alert'
  - name: 'slack'
    slack_config:
    - api_url: 'https://hooks.slack.com/services/TOKEN'
      channel: '#warnings'
```

Validate config file:

```bash
./amtool check-config alertmanager.yml
```

Edit `/opt/prometheus/prometheus.yml`:

```yml
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093
```