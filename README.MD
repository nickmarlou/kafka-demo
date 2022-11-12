# protobuf-demo

## Kafka 101

### Components

#### Zookeper

[Apache ZooKeeper](https://zookeeper.apache.org) is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. Apache Kafka uses Zookeeper to store metadata about the Kafka cluster, as well as consumer client details.

#### Kafka

[Apache Kafka](https://kafka.apache.org) is an open-source distributed event streaming platform. A single Kafka server is called a *Kafka Broker*. An ensemble of Kafka brokers working together is called a *Kafka cluster*.

##### Configuration

**Broker**

| Parameter                           | Description                                                                                    |
| ----------------------------------- | ---------------------------------------------------------------------------------------------- |
| `broker.id`                         | Integer identifier of the broker (0 by default). Must be unique within a single cluster.       |
| `port`                              | By default Kafka starts with a listener on port 9092. It can be set to any other.              |
| `zookeeper.connect`                 | Semicolon-separated list of `hostname:port/path` strings specifying pointing to the Zookeeper. |
| `log.dirs`                          | Path to directories where all messages (log segments) are stored.                              |
| `num.recovery.threads.per.data.dir` | By default, only one thread per directory is used. Set more if case of complex recovering.     |
| `auto.create.topics.enable`         | By default, broker automatically creates a topic. Use `false` to manage topic manually.        |

**Topic**

| Parameter             | Description                                                                               |
| --------------------- | ----------------------------------------------------------------------------------------- |
| `num.paritions`       | Determine how many partitions a new topic is created with. By default it's one partition. |
| `log.retention.ms`    | Main way to retain messages is by time. By default, it's one week (168 hours).            |
| `log.retention.bytes` | Second way to retain messages is by the total number of bytes per-partition.              |
| `log.segment.bytes`   | Once a log segment has reached that size (1GB by default), it'll be closed.               |
| `log.segment.ms`      | Once a log segment size limit is reached, it'll be closed.                                |
| `message.max.bytes`   | Limits the maximum size of a message that can be produced. By default, it's 1MB.          |
