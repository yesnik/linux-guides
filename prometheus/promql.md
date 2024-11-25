# PromQL

## Examples

- `go_info`
- `up`
- `up{job="prometheus"}` - filter metrics by job name
- `node_disk_written_bytes_total{env="dev"}[5m]` - with tag `env="dev"` for last 5 minutes
- `node_network_receive_bytes_total{device="eth0", instance="localhost:9100"}` - show amount of received bytes for device `eth1`, instance `localhost:9100`
