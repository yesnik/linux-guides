# Postgres Exporter

Link: https://github.com/prometheus-community/postgres_exporter

```bash
cd /opt
wget https://github.com/prometheus-community/postgres_exporter/releases/download/v0.13.2/postgres_exporter-0.13.2.linux-amd64.tar.gz
tar zxvf postgres_exporter-0.13.2.linux-amd64.tar.gz
mv postgres_exporter-0.13.2.linux-amd64 postgres_exporter
cd postgres_exporter
```

Create user for exporter:

```bash
su - postgres
psql

create user exporter with password '123';
alter user exporter superuser;
```

Run command with the value of environment variable `DATA_SOURCE_NAME`:

```
DATA_SOURCE_NAME="postgresql://exporter:123@localhost:5432/postgres?sslmode=disable" ./postgres_exporter
```

Access metrics: http://123.123.194.14:9187/metrics

Edit `prometheus.yml`:

```yml
scrape_configs:
  - job_name: 'postgres'
    static_configs:
    - targets: ['localhost:9187']
```
