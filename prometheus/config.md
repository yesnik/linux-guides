# Prometheus Config params

See [official docs](https://prometheus.io/docs/prometheus/latest/configuration/configuration/) about Prometheus Configuration.

We can run `./prometheus` binary with different params:

```
./prometheus --help
```

For example:

- `--config.file="prometheus.yml"` - path to config file
- `--web.listen-address="0.0.0.0:9090"` - address and port for internal web server
- `--web.enable-admin-api` - enable admin API from web interface
- `--web.console.templates="consoles"` - path to html templates
- `--web.console.libraries="console_libraries"` - path to libs for html templates
- `--web.page-title` - web page title
- `--web.cors.origin=".*"` - CORS config for web interface
- `--storage.tsdb.path="data/"` - path for storing time series database data
- `--storage.tsdb.retention.time` - time for storing metrics. Default is 15 days. Expired data will be deleted
- `--storage.tsdb.retention.size` - max size of TSDB. After reaching this value Prometheus will delete old data
- `--query.timeout=4m` - max execution time of one query
- `--enable-feature` - enable [features](https://prometheus.io/docs/prometheus/latest/feature_flags/)
- `--log.level` - logging level

Actual config params we can see in Prometheus Web Interface, at menu *Status - Command-Line Flags*

### Check syntax of prometheus.yml

```bash
./promtool check config prometheus.yml
# Checking prometheus.yml
#  SUCCESS: prometheus.yml is valid prometheus config file syntax
```
