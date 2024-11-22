# Prometheus Nginxlog Exporter

Link: https://github.com/martin-helmich/prometheus-nginxlog-exporter

Exports metrics from Nginx access log files to Prometheus.

See: [releases page](https://github.com/martin-helmich/prometheus-nginxlog-exporter/releases)

```bash
cd /opt
wget https://github.com/martin-helmich/prometheus-nginxlog-exporter/releases/download/v1.11.0/prometheus-nginxlog-exporter_1.11.0_linux_amd64.deb
dpkg -i prometheus-nginxlog-exporter_1.11.0_linux_amd64.deb
```
Edit config: `/etc/prometheus-nginxlog-exporter.hcl`

Commands:
```bash
# Manual start (for debug)
prometheus-nginxlog-exporter -config-file /etc/prometheus-nginxlog-exporter.hcl

systemctl start prometheus-nginxlog-exporter.service
systemctl status prometheus-nginxlog-exporter.service
```
