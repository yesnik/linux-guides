# PromQL

## Examples

- `go_info`
- `up`
- `up{job="prometheus"}` - filter metrics by job name
- `node_disk_written_bytes_total{env="dev"}[5m]` - with tag `env="dev"` for last 5 minutes
- `node_network_receive_bytes_total` - show amount of received bytes
