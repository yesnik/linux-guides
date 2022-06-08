# Apache Kafka

[Apache Kafka](https://kafka.apache.org/) is a community distributed event streaming platform capable of handling trillions of events a day.

## Installation

See [official docs](https://kafka.apache.org/quickstart).

*Note*: Your local environment must have Java 8+ installed:
- check java version: `java -version`
- install java 11: `apt install openjdk-11-jdk`

1. Create `kafka` user:
    ```
    useradd -s /bin/bash -m kafka
    ```
2. Add `kafka` user to `sudo` group:
    ```
    adduser kafka sudo
    ```
3. Login as user kafka: `su - kafka`. 
   Now that we've created a Kafka-specific user, we are ready to download and extract the Kafka binaries.
4. Download binary from the [official site](https://kafka.apache.org/downloads). 
    ```
    wget https://dlcdn.apache.org/kafka/3.2.0/kafka_2.13-3.2.0.tgz
    ```
    **Important**: Download binary (kafka_2.13-3.2.0.tgz), not source files (kafka-3.1.0-src.tgz)
5. Unpack the archive:
    ```
    tar zxvf kafka_2.13-3.2.0.tgz
    ```
6. Start the ZooKeeper service:
    ```
    # Note: Soon, ZooKeeper will no longer be required by Apache Kafka.
    ./bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
    ```
7. Start the Kafka broker service:
    ```
    ./bin/kafka-server-start.sh -daemon config/server.properties
    ```
    Note: We can ommit `-daemon` flag to see logs in the console.

## Config

File: `kafka/config/server.properties`:

- `broker.id=0` - The id of the broker. This must be set to a unique integer for each broker.
- `log.dirs=/tmp/kafka-logs` - A comma separated list of directories under which to store log files.
- `num.partitions=1` - The default number of log partitions per topic
- `log.retention.hours=168` -  The minimum age of a log file to be eligible for deletion due to age. 168 hours is 7 days.
- `log.segment.bytes=1073741824` -  The maximum size of a log segment file. When this size is reached a new log segment will be created.
- `zookeeper.connect=localhost:2181` - Zookeeper connection string

## Create a topic

Before we can write our events, we must create a topic:

```
./bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

## List topics

```
./bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

## Show info about topic

```bash
./bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
# Topic: quickstart-events        TopicId: nWHfsN6LQgOr3q_2fY9aMQ PartitionCount:
# 1       ReplicationFactor: 1    Configs: segment.bytes=1073741824
#         Topic: quickstart-events        Partition: 0    Leader: 0       Replicas
# : 0     Isr: 0
```
