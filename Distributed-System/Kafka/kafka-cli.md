# kafka cli 

## boostrap kafka cluster

1. modify `dataDir` in `zookeeper.properties`
2. modify `log.dirs` in `server.properties`
3. start zookeeper `bin/zookeeper-server-start.sh config/zookeeper.properties`
4. start kafka `bin/kafka-server-start.sh config/server.properties`

## Topic

```bash
# create topic
bin/kafka-topics.sh --zookeeper localhost:2181 --topic first-topic --create --partitions 3 --replication-factor 1

# list topics
bin/kafka-topics.sh --zookeeper localhost:2181 --list

# descibe topic
bin/kafka-topics.sh --zookeeper localhost:2181 --topic first-topic --describe

# delete topic
bin/kafka-topics.sh --zookeeper localhost:2181 --topic first_topic --delete
```

## Producer

```bash
# send message
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic first-topic
>longjie
>nihao
>haha
>^C%

# produce with property
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic first-topic --producer-property acks=all

# send message to un-exist topic will create the topic with default number of partition
```

## Consumer

```bash
# listen to new message of topic
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first-topic

# listen message from beginning
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first-topic --from-beginning

# join a consumer group
# consumer will never listen on the same message
# if consumer all dies all of a sudden and rejoin later, then consumer will catch up the 
# missing message from where it left. since the offset of consumer group is commited to kafka
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first-topic --group app1
```

### Consumer Broup

```bash
# list all consumer groups
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

# describe conumser group
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group app1

# reset offset for all topics
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --group app1 --reset-offsets --to-earliest --execute --all-topics

# reset offset for one topic
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --group app1 --reset-offsets --to-earliest --execute --topic second-topic

# reset offset by shifting n offset on each parition
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --group app1 --reset-offsets --shift-by -3 --execute --all-topics
```
