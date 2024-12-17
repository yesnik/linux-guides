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
