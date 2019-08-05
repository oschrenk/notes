# Kafka

*Apache Kafka* is a /distributed streaming platform/.

A streaming platform has three key capabilities:
* Publish and subscribe to streams of records, similar to a message queue or enterprise messaging system.
* Store streams of records in a fault-tolerant durable way.
* Process streams of records as they occur.

*As a Messaging System*
* Messaging traditionally has two models: [queuing](http://en.wikipedia.org/wiki/Message_queue) and [publish-subscribe](http://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern)
* Queue: a pool of consumers may read from a server and each record goes to one of them
* Publish-Subscribe: the record is broadcast to all consumers

*Concepts*:
* Kafka is run as a cluster on one or more servers that can span multiple datacenters.
* The Kafka cluster stores streams of/records/in categories called/topics/.
* Each record consists of a key, a value, and a timestamp.

Kafka has four core APIs:
* The [Producer API](https://kafka.apache.org/documentation.html#producerapi) allows an application to publish a stream of records to one or more Kafka topics.
* The [Consumer API](https://kafka.apache.org/documentation.html#consumerapi) allows an application to subscribe to one or more topics and process the stream of records produced to them.
* The [Streams API](https://kafka.apache.org/documentation/streams) allows an application to act as a/stream processor/, consuming an input stream from one or more topics and producing an output stream to one or more output topics
* The [Connector API](https://kafka.apache.org/documentation.html#connect) allows to connect Kafka topics to existing applications or data systems. For example, a connector to a relational database might capture every change to a table

## Why Kafka
* Kafka clusters retain all published records. It is by default*persistent —*If you don’t set a limit for Kafka, it will keep records until it runs out of disk space.
* *Multiple Topic Subscriber*. As long as the consumers are part of different groups. If they re the same group (by `group.id`) the data will be balanced.

## Why Not Kafka
Why not go for kinesis or other streaming services?

A: compability with upcoming solutions
A: Third party tooling

## Painpoints
* Zookeeper based
* Max message size 10KB (??? source? Is that still true)
Apache Kafka communication protocol is TCP based

## Contrapoints
We are planning to use Managed Kafka by AWS

## Topics
* topic is a category or feed name to which records are published. Topics in Kafka are *always multi-subscriber*; that is, a topic can have zero, one, or many consumers that subscribe to the data written to it.
* For each topic, the Kafka cluster maintains a partitioned log
* Each partition is an ordered, immutable sequence of records that is continually appended to—a structured commit log.
* The records in the partitions are each assigned a sequential id number called the/offset/that uniquely identifies each record within the partition.

## Guarentees
See  [Guarantees](http://kafka.apache.org/documentation/#intro_guarantees)

* Messages sent by a producer to a particular topic partition will be appended in the order they are sent. That is, if a record M1 is sent by the same producer as a record M2, and M1 is sent first, then M1 will have a lower offset than M2 and appear earlier in the log.
* A consumer instance sees records in the order they are stored in the log.
* For a topic with replication factor N, we will tolerate up to N-1 server failures without losing any records committed to the log.


## Delivery Semantics
* /At most once/—Messages may be lost but are never redelivered.
* /At least once/—Messages are never lost but may be redelivered.
* /Exactly once/—this is what people actually want, each message is delivered once and only once.

## AWS Managed Kafka

Lives in the private IP range in your VPC

How:
1. Go to https://console.aws.amazon.com/msk/home?region=us-east-1
2. Choose a name for your cluster
3. , the VPC you want to run the cluster with,
4.  a data replication strategy for the cluster (three AZ is default for high durability), and the subnets for each AZ.
5. Next, pick a broker instance type and
6. quantity of brokers per AZ, and
7. click create.

you can create topics using the Apache Kafka APIs. All topic and partition level actions and configurations are performed using Apache Kafka APIs.

Amazon MSK does not support public endpoints. It is always over a private connection between your clients in your VPC.

### Reasons

### Limitations
Tools that upload .jar files into Apache Kafka clusters
Data in Flight is not encrypted
it seems that everybody can read/write



### Questions
How to manage schema ? MSK does not have registry
How to backup data? Eg to S3
Can I secure access?
Do we need to secure access?
How to do topic management?
How do you scale the cluster if needed? Does AWS solve rebalancing for us?
How is the support?

## Ideas
Use Kafka Streams to aggregate

## Glossary
*Producer*
Producers publish data to the topics of their choice. Producer is responsible for choosing which record to assign to which partition within the topic. This can be done in a round-robin fashion simply to balance load or it can be done according to some semantic partition function (say based on some key in the record)

*Consumer*
Consumers label themselves with a/consumer group/name, and each record published to a topic is delivered to one consumer instance within each subscribing consumer group.

*Topic*

*Kafka topic partition*
A*Topic*is a category/feed name to which messages are stored and published. Messages are byte arrays that can store any object in any format.

*Order*
Kafka only provides a total order over records/within/a partition, not between different partitions in a topic. Per-partition ordering combined with the ability to partition data by key is sufficient for most applications. However, if you require a total order over records this can be achieved with a topic that has only one partition, though this will mean only one consumer process per consumer group.

*Broker*
Each node in the cluster is called a Kafka/broker/. Each broker holds a number of partitions and each of these partitions can be either a leader or a replica for a topic.

*Leader*
All writes and reads to a topic go through the leader and the leader coordinates updating replicas with new data. If a leader fails, a replica takes over as the new leader.

*Replica*
A replica of a partition is a “backup” of a partition. Replicas never read or write data. They are used to prevent data loss.

*Offset*
The offset is a unique identifier of a record within a partition. It denotes the position of the consumer in the partition.

*Node:*
A node is a single computer in the Apache Kafka cluster.

*Cluster:*
A cluster is a group of nodes i.e., a group of computers.

