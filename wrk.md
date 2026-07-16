# Wrk

[wrk](https://github.com/wg/wrk) is a HTTP benchmarking tool

## Installation

Ubuntu:

```
apt install wrk
```

## Examples

```
wrk -t12 -c400 -d30s https://mysite.com/
```
- `t12` - 12 threads
- `c400` - 400 connections
- `d30s` - test duration 30s
