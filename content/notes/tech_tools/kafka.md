+++
title = 'Apache Kafka'
date = 2024-02-11

+++

- When we create a topic we have to say how many partitions we need and replication-factor
- In kafka we can't create a topic with replication-factor > no. of brokers we have
- `kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor 1`
- `kafka-topics.sh --zookeeper 127.0.0.1:2181 --list`
- `kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --describe`
- `kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic second_topic --delete`
- `kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic first_topic` : to start the console for producing the data
- while producing if a topic doesn't exist it gets created. default partition count = 1 and replication_factor = 1
- we can set `num.partitions=3` in `server.properties` files so that when a topic doesn't exist it gets created with that many partitions
- best practice is to create a topic before producing to it
- kafka-console-consumer does not read all the topics. It only reads from the point we launch it and will only intercept new messages
- `kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic`
- `kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning` reads all the message from beginning
- `kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application` : starts a consumer with consumer group my-first-application
- 3 partitions + 2 console consumer sharing the same group
- 3 partition + 3 consumer : each consumer consumes from different partition
- `kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-second-application --from-beginning` : it will first read all the message from the topic and then commit the offset. If we start the same command again it will wait for new messages.
- If we stop the consumer and send message via the producer and then restart the consumer it will read the messages sent after it was first stopped. As it has already read the messages before stopping it picks after that.
- `kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list`
- `kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-second-application` : lag = 0 means that yes indeed it has read all the data
- We can run the kafka-console-consumer command and side by side kafka-consumer-groups command and see which machines/applications are consuming from a topic
- `bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --execute --topic first_topic` to reset offset to the beginning
- `bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -2 --execute --topic first_topic` shift left by 2
- Producer with keys

```bash
kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic --property parse.key=true --property key.separator=,
> key,value
> another key,another value
```

- Consumer with keys :

```bash
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning --property print.key=true --property key.separator=,`
- `kafka-console-producer
```

- CLi needs to connect to --broker-list
- Installing conduktor

```
$ curl -LO http://mirrors.kernel.org/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
$ sudo dpkg -i libffi6_3.2.1-9_amd64.deb
$ sudo dpkg -i Conduktor-2.7.0.deb
```

### Java Coding

- Key serializer and value serializer help the producer know what type of value you're sending to Kafka and how this should be serialized to bytes.
- By providing a key we guarantee that the same key always goes to the same partition.
- By changing the group Id you actually reset an application
- [Squeezing the firehose: getting the most from Kafka compression](https://blog.cloudflare.com/squeezing-the-firehose/)

### Advanced

- `kafka-configs.sh --zookeeper localhost:2181 --entity-type topics --entity-name configured-topic --describe`
- `kafka-configs.sh --bootstrap-server localhost:2181 --entity-type topics --entity-name configured-topic --describe`
- `kafka-configs.sh --zookeeper localhost:2181 --entity-type topics --entity-name configured-topic --add-config min.insync.replicas=2 --alter` : to add a config
- `kafka-configs.sh --zookeeper localhost:2181 --entity-type topics --entity-name configured-topic --delete-config min.insync.replicas --alter` : for deleting a config
- .log file is the segment

### Starting Kafka other ways

- `./confluent local start`
- `./confluent local stop`
- `./confluent local destroy`
- `./confluent local start kafka` : just start zk and kafka
- starting a multibroker kafka cluster:
- [Running kafka on docker using compose](https://github.com/simplesteph/kafka-stack-docker-compose)
- advertised.listeners changed to public IP so that we can run kafka on EC2 and connect to it

### From the blogs

- You try to send two records to Kafka. One is sent successfully, but a network issue causes the other to fail. You try to resend the data, but now you have two active requests at the same time, creating the possibility that the data will be stored out of order.
- The solution is to set Kafka’s `max.in.flight.requests.per.connection ` parameter to 1. When this is set higher than one, and the retries parameter is above zero, the broker can fail at writing the first message batch but succeed at the second that was in-flight at the same time, then retry and succeed at writing the first message (switching their stored order). By setting max.in.flight.requests.per.connection to 1, you can make sure those requests are completed sequentially, in your intended order.
- If data order is absolutely critical to your application, I also recommend setting in.flight.requests.per.session to 1. This tells Kafka not to send any other messages until the current message batch is complete. However, this technique comes with severe costs to producer throughput, and is only appropriate for use cases where data order is crucially important. Also, beware of the temptation to set allowed retries to zero; doing so has a devastating effect on system reliability.

### KSQL

- create stream users_stream (name VARCHAR, countrycode VARCHAR) with (KAFKA_TOPIC='USERS', value_format='DELIMITED');
- list topics; show topics;
- list streams; or show streams;
- Offset reset setting.
- SET 'auto.offset.reset'='earliest';
- select name, countrycode from users_stream emit changes;
- drop stream if exists users_stream delete topic; (also deletes the topic)
- create stream userprofile (userid INT, firstname varchar, lastname varchar, countrycode varchar,rating double) with (value_format='JSON', kafka_topic='USERPROFILE');
- describe $stream;
- ksql-datagen schema=./datagen/userprofile.avro format=json topic=USERPROFILE key=userid maxInterval=5000 iterations=100

### Notes

- Kafka server/broker. It is what users interact with.
- Kafka broker (9092) default port
- Bi-directional connection between producer and broker and broker and consumer
- Topics are basically the logical partitions where you write data
- Producers write to topcis
- Producer says : Publish "John" to "Users" topic, appends it to Users topic.
- Each message is referred to by topic and position.
- Consumers say hey we wanna consume topic Users. Consumers is polling for information (asking for more)
- As topics get full, it partitions the topic similar to sharding.
- Publisher has to figure out which partition to write to.
- Consume Users topic on partition 2 position 0
- To do both Pub/sub and Queue Kafka relies on an abstraction. That abstraction is called **consumer group**
- Consumer groups were invented to do parallel processing on partitions.
- When there's only one consumer in the consumer group it has to consume from all the partition of a particular topic. However, when another consumer is added to that group the load rebalances.
- Each partition has to be consumed by one and only one consumer in a consumer group. However a consumer can consume from multiple partitions.
- To act like a queue, put all your consumers in one group
- To act like a pub/sub, put each consumer in a unique group.
- Append only commit log.
- It is event driven.

### Optimizing Producers

- *Data durability* to minimize data loss when passing messages
  - Specify `acks=all` to force a partition leader to replicate messages to a certain no. of followers before acknowledging that the message request was successfully received.
  - Use the `acks=all` in conjunction with the `min.insync.replicas` property for topics.
  - `min.insync.replicas` sets the number of **brokers** that need to have logged a message before an ack. is sent to the producer.
  - Because of additional checks, `acks=all` increases the latency between producer sending a message and receiving ack.

- *Ordered delivery*
  - There are two approaches to guaranteeing the order of message delivery from producers. And they depend, to a large degree, on whether or not you are using `acks=all`
  - If using `acks=all`, you can (and should) enable idempotence using `enable.idempotence` for the producer to ensure that messages are delivered only once.
  - To speed the process along, increase the number of in-flight requests at one time using `max.in.flight.requests.per.connection`
  - `delivery.timeout.ms` sets a limit on the time to wait for an ack. of the success or failure to deliver a message.
  - `retries` property sets the number of retries when resending a failed message request.
  - If not using `acks=all` and idempotency, another option is to set the number of in-flight requests to 1 (default is 5) to preserve ordering.

- *Reliability guaranatees* on message transactions involving batches of related messages
  - How do we guarantee the reliability of message delivery for exactly once writes for a set of messages across multiple partitions? We use idempotence again but combine it with a unique transactional ID defined for the producer.
  - Specify a unique transaction ID in the producer configuration - `transactional.id=_UNIQUE-ID_`, and also set the maximum allowed time for transactions in ms - `transaction.timeout.ms`. The default is `15min`

- *Throughput* measured in the number of messages processed over a specific period
- *Latency* measured in the time it takes for messages to reach the broker
- It's quite likely you'll want to balance throughput and latency targets whilst also minimizing data loss and guaranteeing ordering.
- Message batching delays sending messages so that more messages destined for the same broker are batched into a single request.
- You can use two properties to set batch thresholds:
  - `batch.size` specifies a maximum batch size in bytes (default 16384/16kb)
  - `linger.ms` specifies a maximum duration to fill the batch in milliseconds (Default 0 or no delay)
- Use `buffer.memory` to configure a buffer memory size that must be at least as big as the batch size, and also capable of accommodating buffering, compression and in-flight requests.
- What is gained in higher throughput is conceded with the buffering that adds higher latency to the message delivery.
- Use the `compression.type` property to specify a valid compression codec. You can choose `gzip`, `snappy`, `lz4`, or `zstd` with each having varying compression speeds.

### Optimizing Consumers

- *Consumer scalability* to accommodate increased throughput
- *Throughput* measured in the number of messages processed over a specific period
  - `fetch.max.wait.ms` sets a maximum threshold for time-based batching 
  - `fetch.min.bytes` sets a minimum threshold for size-based batching
  - Adjust the properties higher so that there are fewer requests, and messages are delivered in bigger batches.

- *Latency* measured in the time it takes for messages to be fetched from the broker
  - `fetch.max.bytes` sets a maximum limit in bytes on the amount of data fetched from the broker at one time.
  - `max.partition.fetch.bytes` sets a max. limit in bytes on how much data is returned for each partition, which must always be larger than the number of bytes set in the broker/topic config for `max.message.bytes`.
  - By increasing the values of these two properties, and allowing more data in each request, latency might be improved as there are fewer fetch requests.

- *Data loss or duplication* when committing offsets or recovering from failure
  - When auto-commit is enabled, consumers commit the offsets of messages automatically every `auto.commit.interval.ms` ms. But with this comes risk of :
  - **Data Loss** : if app commits an offset, and the crashes before all the messages up to that offset have actually been processed, those messages won't get processed when the app restarts.
  - **Data duplication** : if the app has processed all the messages, and then crashes before the offset is committed, the last offset is used when the app restarts and it processes the same messages again.
  - We can do manual commits by calls to `commitSync` and `commitAsync` APIs
  - `commitSync` API commits the offsets of all messages returned from polling.
  - `commitAsync` API doesn't wait for the broker to respond to a commit request. It has lower latency than `commitSync` API, but risks creating dups when rebalancing.
  - Manual commits cannot completely eliminate data duplication because you cannot guarantee that the offset commit message will always be processed by the broker; even with synchronous commit.

- *Handling of transactional messages* from the producer and consumer side
  - If producers are configured to use idempotence and transactional ids, we can make the pipeline more secure from the consumer side by introducing the `isolation.level` property.
  - `isolation.level` property controls how transactional messages are read by the consumer, and has two valid values:
    - `read_committed` - only transactional messages that have been committed are read by the consumer. It leads to an increase in latency.
    - `read_uncommitted` (default)

- *Minimizing the impact of rebalances* to reduce downtime
  - `session.timeout.ms` specifies the max. amount of time in ms. a consumer within a consumer group can be out of contact with a broker before being considered inactive and a _rebalancing_ is triggered b/w the active consumers in the group.
  - `heartbeat.interval.ms` specifies the interval in ms. b/w _heartbeat_ checks to the consumer group coordinator to indicate that a consumer is active and connected. It must be lower, usually by a third, than the session timeout interval.
  - If the broker configuration specifies a `group.min.session.timeout.ms` and `group.max.session.timeout.ms`, then `session.timeout.ms` value must be within that range.

- We want to achieve a balance b/w throughput and latency and ensure that the configuration helps reduce delays caused by unnecessary rebalances of consumer groups.
- Kafka _only_ provides ordering guarantees for messages in a single partition.
- Consumer groups are way of sharing the work of consuming messages from a set of partitions b/w a number of consumers by dividing the partitions b/w them.
- Consumers within a group do not read data from the same partition, but can receive data exclusively from zero or more partitions.
- Adding more consumers than partitions will not increase throughput. Not entirely pointless, as an idle consumer is effectively on standby in the event of failure of one of the consumers that does have partitions assigned.
- `auto.offset.reset` set as `latest`, which is the default, a new consumer will start processing only new messages and if set to `earliest`, it will processes messages starting from the beginning of the log.
- `group.instance.id` property is used to specify a unique group instance id for a consumer. The consumer group coordinator can then use the id when identifying a new consumer instance following a restart.
- `max.poll.interval.ms` sets the interval to check the consumer is continuing to process messages. If the consumer app doesn't make a call to poll at least every `max.poll.interval.ms` ms., the consumer is considered to be failed, causing a rebalance.
- `max.poll.records` set the number of processed records returned from the consumer. Use this property to set a max. limit on the number of records returned from the consumer buffer, allowing the app to process fewer records within `max.poll.interval.ms` limit.
