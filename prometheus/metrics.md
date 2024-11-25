# Prometheus Server Metrics

Access Prometheus server metrics from localhost:

- Open Web Browser: http://123.123.123.123:9090/metrics
- Use curl: `curl localhost:9090/metrics`

### Metrics Examples

- `node_disk_read_bytes_total` - bytes that were read from the disk
- `node_disk_read_time_seconds_total`
- `node_disk_reads_completed_total` - disk reads count
- `process_resident_memory_bytes` - the physical memory occupied by the Prometheus process.

### Metrics Format

*Prometheus metric* is just a string with metric's name, tags and value:

```
node_cpu_seconds_total{cpu="0",mode="user"} 54.15
```

Here:

- `node_cpu_seconds_total` - metric name
- `cpu="0", mode="user"` - tags. We can add tag `{env="prod"}` or `{env="dev"}` to filter metrics of Test and Prod environments
- `54.15` - metric value
