# Apache Kafka

[Apache Kafka](https://kafka.apache.org/) is a community distributed event streaming platform capable of handling trillions of events a day.

## Installation

See [official docs](https://kafka.apache.org/quickstart)

1. Create kafka user:
  ```
  useradd -s /bin/bash -m kafka
  ```
2. Login as user kafka: `su - kafka`
3. Download .tgz archive from the [official site](https://kafka.apache.org/downloads)
  ```
  wget https://dlcdn.apache.org/kafka/3.1.0/kafka-3.1.0-src.tgz
  ```
4. Unpack the archive:
  ```
  tar zxvf kafka-3.1.0-src.tgz
  ```

## Config

File: `kafka/config/server.properties`:

- `broker.id=0` - The id of the broker. This must be set to a unique integer for each broker.
- `log.dirs=/tmp/kafka-logs` - A comma separated list of directories under which to store log files.
- `num.partitions=1` - The default number of log partitions per topic
- `log.retention.hours=168` -  The minimum age of a log file to be eligible for deletion due to age. 168 hours is 7 days.
- `log.segment.bytes=1073741824` -  The maximum size of a log segment file. When this size is reached a new log segment will be created.
- `zookeeper.connect=localhost:2181` - Zookeeper connection string
