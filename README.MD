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

### Hardware Selection

Once performance becomes a concern, there are several factors that will contribute to it: 

- disk throughput,
- disk capacity,
- memory,
- networking,
- CPU.

The performance of producer clients most directly influenced by the **disk throughput**. Kafka message must be committed by at least one broker before considering the send successful. This means *the faster disk the lower produce latency*.

The amount of **disk capacity** that is needed is determined by how many messages need to be retained. You should also factor in at least 10% overhead for other files, in addition to any buffer that you wish to maintain for fluctuations in traffic or growth over time.

Kafka itself does not need much heap **memory** configured for the JVM. For most of the cases 5GB is enough. The rest will be used by the page cache. Having more memory available to the system for [page cache](https://en.wikipedia.org/wiki/Page_cache) will improve the performance of consumer clients by allowing the system to cache log segments in use.

The available **network throughput** will specify the maximum amount of traffic that Kafka can handle. This is complicated by the inherent imbalance between inbound and outbound network usage that is created by Kafka’s support for multiple consumers.

**CPU** is not as important as disk and memory and should not be the primary factor in selecting hardware. The majority of Kafka’s requirement for processing power comes from decompressing and recompressing messages, alongside checksum validation.

#### AWS

For [AWS EC2](https://aws.amazon.com/ru/ec2/instance-types) either the `m4` or `r3` instance types are a common choice. 

The `m4` instance will allow for *greater retention periods, but the throughput to the disk will be less* because it is on elastic block storage. 

The `r3` instance will have *much better throughput* with local SSD drives, but those drives will *limit the amount of data that can be retained*. 

For the best of both worlds, it is necessary to move up to either the `i2` or `d2` instance types, which are significantly more expensive.
