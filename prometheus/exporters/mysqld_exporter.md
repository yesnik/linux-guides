# Mysqld Exporter

Link: https://github.com/prometheus/mysqld_exporter

```bash
cd /opt
wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.15.0/mysqld_exporter-0.15.0.linux-amd64.tar.gz
tar xzvf mysqld_exporter-0.15.0.linux-amd64.tar.gz
mv mysqld_exporter-0.15.0.linux-amd64 mysqld_exporter
cd mysqld_exporter
```

Create user in MySQL for exporter:

```
CREATE USER 'exporter'@'localhost' IDENTIFIED BY '123' WITH MAX_USER_CONNECTIONS 3;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
FLUSH PRIVILEGES;
```

File `/opt/mysqld_exporter/.my.cnf`:

```
[client]
user=exporter
password=123
```

File `/etc/systemd/system/mysqld_exporter.service`:

```
[Unit]
Description=Mysqld Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/opt/mysqld_exporter/mysqld_exporter \
    --config.my-cnf /opt/mysqld_exporter/.my.cnf \
    --collect.global_status \
    --collect.info_schema.innodb_metrics \
    --collect.auto_increment.columns \
    --collect.info_schema.processlist \
    --collect.binlog_size \
    --collect.info_schema.tablestats \
    --collect.global_variables \
    --collect.info_schema.query_response_time \
    --collect.info_schema.userstats \
    --collect.info_schema.tables \
    --collect.perf_schema.tablelocks \
    --collect.perf_schema.file_events \
    --collect.perf_schema.eventswaits \
    --collect.perf_schema.indexiowaits \
    --collect.perf_schema.tableiowaits \
    --collect.slave_status \
    --web.listen-address=0.0.0.0:9111

[Install]
WantedBy=multi-user.target
```

Edit `prometheus.yml`:

```
scrape_configs:
  - job_name: 'mysql'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9111']
```
