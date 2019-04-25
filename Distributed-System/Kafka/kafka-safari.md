# Apache Kafka Series - Learn Apache Kafka for Beginners

## Topics

- similiar to a table in database
- topics are split in partitions
- message is ordered within each partition
- offset: incremental id of message within each partition
- message is only kepy for a limited of time(default is one week)
- message is immutable
- message is randomly assigned to parition unless using a key

## Broker

- kafka cluster is composed of multiple brokers
- each broker is identified with ID
- each broker contains certain topic partition 
- every broker is called `bootstrap server`, Each broker know about metadata of all brokers, topic and partitions. That means after connecting to any broker, you will be connected to the entire cluster. 

## replication factor

- at any time, only one broker can be the *leader* for a given partition. Only the leader can receive and serve the data the given partition. other brokers will synchronize the data. Therefore each partition has one leader and multipel ISR(in-sync replica). 
- zookeeper handles the leader and ISR of given partition
- if leader is down, one ISR will become leader after election
- With a replication factor of N, producers and consumers can tolerate up to N - 1 brokers being down

## Producer

- producers write data to topics
- producers automatically knows which broker and partition to write to
- producer can choose to receive acknowledgement of data writes(confirmation):
  - acks=0. no wait. possible data loss.
  - acks=1. wait for leader's ackowledgement, limited data loss.
  - acks=all, wait for leader + replica. no data loss.
- produce can send key with message
  - if key is null, data is sent by round robin
  - if key is provided, the message with same key goes for same partition
  - a key is used when you need ordering for specific message.

## Consumer

- consumers read data from a topic
- consumers automatically knows which broker to read from
- data is read in order within each partitions

### Consumer group

- counsumer groups consist of multiple consumer
- consumers from group read from exclusive(diffent) partition.
- if there are more consumers than partitions, some consumer is inactive.

### consumer group offsets

- kafka store the offset at which a consumer group has been reading
- when a consumer in a `consumer group` processed data received from kafka, it should be committing the offset.
- if a consumer dies, it is able to read back from where it left off.

#### message delivery semantics

consumer choose when to commit offset

- At most once: committed as soon as the message is received, if processing goes wrong, the message will be lost
- At least once: committed when message is processed, message can be processed multiple time, make sure system processing is idempotent.
- Exactly once: can be achieved kafka to kafka with stram API. 

## Zookeeper in Kafka

- manages brokers
- helps in performing leader election for partitions
- send notification to Kafka in case of changes(new topic, broker down/up, delete topic..)
- zookeeper operate with odd number of servers
- zookeeper leader handler write, follower handle read