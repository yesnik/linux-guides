# Prometheus Backups

## Way 1

We can have 2 Prometheus instances that uses the same config file. If one instance is damaged we have another instance as a backup.

## Way 2

We can enable [TSDB Admin API](https://prometheus.io/docs/prometheus/latest/querying/api/#tsdb-admin-apis) - pass option `--web.enable-admin-api` when starting Prometheus.

After this we can make a HTTP request:

```
curl -XPOST http://localhost:9090/api/v1/admin/tsdb/snapshot
{
  "status": "success",
  "data": {
    "name": "20171210T211224Z-2be650b6d019eb54"
  }
}
```

The snapshot now exists at `<data-dir>/snapshots/20171210T211224Z-2be650b6d019eb54`
