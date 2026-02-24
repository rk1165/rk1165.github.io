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
