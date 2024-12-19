# Prometheus Integrations

See [docs](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage)

## Remote Endpoints and Storage

The [remote write](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_write) and remote read features of Prometheus allow transparently sending and receiving samples. 
This is primarily intended for long term storage.

Examples:

- Prometheus -> Adapter (prometheus-kafka-adapter) -> Kafka ( ~200k inserts in topic / sec ) -> Clickhouse
- Prometheus -> Adapter -> Kafka -> Hadoop
- Prometheus -> Adapter (Promscale) -> Postgres

### prometheus-kafka-adapter

[prometheus-kafka-adapter](https://github.com/Telefonica/prometheus-kafka-adapter) allows to use Kafka as a remote storage database for Prometheus (remote write only).
