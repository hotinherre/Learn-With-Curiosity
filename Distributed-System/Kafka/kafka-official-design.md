# Kafka Official Document - Design

## persistence

- In fact disk is not that slow. Sequential disk access can be 6000x faster than random access.
- Usually OS will free memory as page cache(cache for disk).
- java memory overhead is very high, often doubling size of data stored. Java gc is fiddly.

The above reason lead to a simple design for Kafka: rather than maintaining data in memory and flushing data into filesystem later, kafka write data to persistent commit log on the filesystem immediately. Then wait for OS to transfer data into page cache.

Varnish also use page-cache-centric design. varnish is `HTTP accelerator` or `HTTP cache`.

## Efficiency

There are two main cause of inefficiency for message streaming system. Here is how Kafka solove it.
1. too many small I/O request - Kafka group small messages together as a batch request. 
2. excessive byte copying and manipulating - kafka use a special binary format for message. The message can be grouped and compressed together and written into file without any modification. The log will only be decompressed by consumer.

## Producer

Any broker knows whole metadata of the entire kafka cluster. Producer only need to specify topic and which partition message go to. If provide a key, message goes to specific partition, otherwise, use round robin. Producer attempt to accumulate data in memory. Batch is configured to accumulate no more than fixed size number of messages and no longer than fixed latency bound.

## Consumer

Consumer fetch logs from broker, broker returns back a chunk of log beginning from the offset consumer specify. Consumer has significant control over offset. 

### why kafka doesn't push message to consumer

1. consumer can be overwhelmed when consumption rate falls behind producing rate
2. To reduce I/O rate, kafka group message together and send to consumer. This will introduce latency. Or kafka can push each message to consumer, this cost too much small i/o request.

### problem with message polling

For normal polling, consumer may end up make many get call with no response. Kafka use long polling instead.

### Consumer Position

Most message system keep track of consumer position in server. When message is sent out, message is marked as sent. When server receive acknowledgement from consumer, message is marked as consumed. Then server should notify consumer the message is marked as consumed. Failure of each step my need retry.

Kafka mark consumer position as offset for each partition and each consumer. It is just an integer. And this design allows consumer to rewind back to old offset

### Offline Data Load

Kafka support offline periodically data consume.

## Message Delivery Semantic

kafka has 3 delivery guarantees:
- at most once - message may be lost, but never redelivered
- at least once - message are never lost, but may be redelivered
- exactly once - each message only deliver once. Kafka support exactly once delivery in kafka streams with new transaction capability. If topic is delivered, then offset is reverted back.

Kafka use at least once by default.

### Guarantee for message publishing

Once kafka message is committed, then it will bot be lost unless all replica dies. In the case of message is being committed in broker but fails sending acknowledgement back, prior to 0.11.0.0, producer has to resend duplicate message and commit to broker again. Since 0.11.0.0, each producer has unique id and each message of one producer has sequential id, kafka broker can deduplicate message of same same produce Id and message id. Also kafka support transaction-like semantics whe send messages to multiple topic.

For latency sensitive system, producer don't need to wait for all replica finish write. 


### Guarantee for message consuming

For at most once, consumer update offset before processing. If consumer crash, then data is lost.  
For at least once, consumer update offset after processing. Data is never lost, but same data may be reprocessed.


## Replication

unit of replication is topic partition. Each parition has one leader and 0+ followers. Read write goes to leader. Leaders are evenly distributed among brokders. The logs on followers suppose to be identical to leaders, there may be some latency, but finally will be the same.Followers consume message from leader by pulling.

In-sync follower node has two conditions:
- maintain session with zookeeper
- it cannot fall too far behind from leader
leader tracks in-sync nodes, if one of them fails, remove it from list.

message is considered committed when all ISR replicate the message. Only commited message are given out to consumers. ISR is persisted in zookeeper.

When all ISR dies:
- choose first ISR comes back, takes long time or maybe forever to recover.
- choose first remaining replica. Some data maybe lost.

two topic-level configuration to choose durability over availability:
- disable unclean leader election. Only ISR can be leadere
- sepcify a minimum ISR size. If there is not enough ISR, log is not commited.

## Log Compaction

log companction is handle by log cleaner in background. It remove the old record with same key, only keey the most recent one.

## Quotas

limit resource for consumer and producer.
- netword bandwith quota
- request rate quota