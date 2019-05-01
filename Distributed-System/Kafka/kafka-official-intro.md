# Kafka Official Document - Intro

kafka is a streaming platform with following capability:

- pub/sub
- store streams of record
- process stream

Kafka is generally used for:

- real-time streaming **data** pipeline
- real-time streaming app react or transform stream of data

kafka topic consists of key, val, timestamp

kafka has 4 apis:

- producer API
- consumer API
- streams API - consume input stream from topics and produce output streams to topics
- connect API - use reusable consumer or producer that connect kafka to external application or data systems. For example, connect from db to kafka.

## Topic

For each topic, kafka maintains a partitioned log. In each partition, log is ordered and assigned a sequential id called offset. Kafka persists all logs with a configurable retention time. After retention time, record is discarded. For each consumer metadata, kafka only saves its offset. Consumer can modify offset in any order they like. The Kafka consumer is cheap by this design, individual consumer won't impact other consumer or kafka.

Topic partition serve two purpose:

- hold arbitrary amount of data
- provide parallelism for consumer

For topic with replication factor N, kafka will tolerate up to N - 1 server failure without losing any record.

## Producer

produce message of topic to partitions:

- round robin
- based on key

## Consumer

consumer label themselves with a consumer group. Consumer of consumer groups listens to topic partitions exclusively. If new instance will take over partitions from other member. If instance dies, its partition is distributed to other member. If you require total order of records, this can be achieved with topic of one partition.

## Kafka as a Messaging System

### Queue

record is saved to queue, consumers read from queue.  
**Pro**: provide parallelism by dividing up data.  
**Cons**: doesn't support multiple subscriber groups, once data is consumed from queue, it is gone.

### Publish-Subscribe

the record is broadcasted to all consumers.
**Pro**: support multiple consumers
**Cons**: no parallelism, cannot scale.

Kafka generalize both concepts of Queue and Pub-Sub. As a queue, consumer group has multiple consumers to divide the input data. As a pub-sub, kafka broadcasts messages to multiple consumer groups.

Kafka has a stronger `message ordering guarantee`. In traditional message queue, consumer read message from queue asynchronously, the time consumer finish the message processing is not guaranteed. Unless you only have one consumer for one queue. Each of Kafka topics' partition is only assigned to one consumer, then the order within partition is guaranteed also multiple partitions allows multiple consumer subscribe for one topic.

## Kafka as a Storage System

Kafka write data to disk and replicate them. Kafka provides write acknowledgement semantics. kafka scales well.

## Kafka for Stream Processing

In Kafka a stream processor is anything that takes continual streams of data from input topics, performs some processing on this input, and produces continual streams of data to output topics.

## Putting together

Distributed file system store files for _historical_ data batch processing.

Traditional message queue allows processing _future/live streaming data_.

Kafka allows applications treating both past and live streaming data the same way. An application can start process the historical stored data, when it reaches last record it can keep processing the future data app rather than ending.