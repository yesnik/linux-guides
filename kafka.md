# Apache Kafka

[Apache Kafka](https://kafka.apache.org/) is a community distributed event streaming platform capable of handling trillions of events a day.

## Installation

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

