# PromQL

## Examples

- `go_info`
- `up`
- `up{job="prometheus"}` - filter metrics by job name
- `up{env!="prod"}` - filter metrics by env
- `up{job=~".*city.*"}` - filter by RegExp
- `http_requests{host="mysite.com", status=~"2.+"}` - filter by RegExp
- `up == 0` - show `up` metrics with 0 value
- `{__name__=~"^redis_.*"}` - filter by metric name - `__name__`
- `{__name__=~"^redis_.*"} > 1000` - filter redis metrics that have value > 1000. This is useful for alerting config.
- `node_disk_written_bytes_total{env="dev"}[5m]` - with tag `env="dev"` for last 5 minutes
- `node_network_receive_bytes_total{device="eth0", instance="localhost:9100"}` - show amount of received bytes for device `eth1`, instance `localhost:9100`

## Data Types

### Scalar

- `up{} + 1 != 0`
- `2 * 2`

We can use arithmetical operations with scalar types. We can get free space percentage: `used space / total space * 100` 

### Instant vector

It's a metric with the last value.

`up`

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

## Operators

- `sum by (env)(up)` - sum all `up` metrics grouped by `env` tag
- `sum by (env)(node_memory_MemTotal_bytes) / 1024 / 1024 / 1024` - show total memory in Gb for `env=prod`, `env=dev`

## Metric types

### Gauge

This metric can change with time. Examples:

- Temperature in the server room
- `node_filesystem_avail_bytes`

### Counter

It can only go up.

- `node_cpu_seconds_total`
- `go_memstats_malloc_total`

### Summary

This metric shows percentiles. It can help us to calculate execution time of our app's queries.

Client app calculates these metrics. It consumes app's server resources. It shows predefined precentiles - `0.5`, `0.9`, `0.99`.

- `prometheus_engine_query_duration_seconds{slice="inner_eval",quantile="0.5"}`

### Histogram

These metrics increas counters for metric's buckets. Prometheus does required calculations.

Histogram help us to calculate any percentile we want.

- `prometheus_http_request_duration_seconds_bucket{handler="/",lte="0.1"}`

## Query Functions

See [docs](https://prometheus.io/docs/prometheus/latest/querying/functions/)

### increase()

The following example expression returns the number of HTTP requests as measured over the last 5 minutes, per time series in the range vector:

```
increase(http_requests_total{job="api-server"}[5m])
```

### rate()

It takes the first and the last value in the provided interval and calculates the per-second average rate of the counter increase. This function works only with counters.

Per-second rate of HTTP requests as measured over the last 5 mins:
```
rate(http_requests_total{job="api-server"}[5m])
```

Per-second rate of execution command `info` as measured over the last 10 mins:
```
rate(redis_commands_total{cmd="info"}[10m])
```

We can emulate load on Redis:

```
for i in {1..100}; do redis-cli info > /dev/null; done
```

### irate()

It takes the last value and the value before the last in the provided interval and do the same as `rate()`.

### delta()

Shows the difference between the last and the first value in the interval.

The difference in CPU temperature between now and 2 hours ago:
```
delta(cpu_temp_celsius{host="zeus"}[2h])
```

### changes()

Returns the number of times its value has changed within the provided time range.

```
changes(up[10m])
```

### histogram_quantile()

Calculates the φ-quantile (0 ≤ φ ≤ 1) from a classic histogram or from a native histogram.

A histogram metric is called `http_request_duration_second`s (and therefore the metric name for the buckets of a classic histogram is `http_request_duration_seconds_bucket`).
To calculate the 90th percentile of request durations over the last `10m`, use the following expression in case `http_request_duration_seconds` is a classic histogram:

```
histogram_quantile(0.9, rate(http_request_duration_seconds_bucket[10m]))
```

For a native histogram, use the following expression instead:

```
histogram_quantile(0.9, rate(http_request_duration_seconds[10m]))
```

Prometheus http request duration, 85th percentile:
```
histogram_quantile(0.85, rate(prometheus_http_request_duration_seconds_bucket[30m]))
```

### predict_linear()

Predicts the value of time series `t` seconds from now, based on the range vector v, using simple linear regression.
The range vector must have at least two samples in order to perform the calculation.
It should only be used with gauges.

```
predict_linear(prometheus_tsdb_blocks_loaded[60m], 60)
```

### sum()

```
sum(up)
sum by (job) (up)
sum by (instance) (rate(prometheus_http_requests_total{}[2m]))
sum by (uri) (rate(http_server_requests_seconds_count{uri=~"/api/.*", status=~"(4|5).*"}[1m]))
```
