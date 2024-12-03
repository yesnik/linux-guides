# PromQL

## Examples

- `go_info`
- `up`
- `up{job="prometheus"}` - filter metrics by job name
- `up{env!="prod"}` - filter metrics by env
- `up{job=~".*city.*"}` - filter by RegExp
- `{__name__=~"^redis_.*"}` - filter by metric name - `__name__`
- `{__name__=~"^redis_.*"} > 1000` - filter redis metrics that have value > 1000. This is useful for alerting config.
- `node_disk_written_bytes_total{env="dev"}[5m]` - with tag `env="dev"` for last 5 minutes
- `node_network_receive_bytes_total{device="eth0", instance="localhost:9100"}` - show amount of received bytes for device `eth1`, instance `localhost:9100`

## Data Types

### Scalar

`up{} + 1 != 0`

We can use arithmetical operations with scalar types. We can get free space percentage: `used space / total space * 100` 

### Instant vector

It's a metric with the last value.

### Range vector

We define a period: `up[1m]`

Result - 4 measurement per 1 min:

```
1 @1733230611.121
1 @1733230626.121
0 @1733230641.121
1 @1733230656.121
```
where `1` or `0` - metric value, `1733230611` - timestamp.

