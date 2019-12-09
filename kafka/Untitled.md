kafka数据的基本单位：message



1. 查看kafka 的topic 
```bash
./kafka-topics.sh --list --zookeeper ZOOKEEPER_HOST:2181
```

2. 创建kafka的topic
```bash
./kafka-topics.sh --create --zookeeper ZOOKEEPER_HOST:2181 --replication-factor REPLIC_NUMBER --partitions PARTITIONS_NUMBER --topic TOPIC_NAME
```
<!--REPLIC_NUMBER  为副本系数。一条数据的备份数。-->
<!--PARTITIONS_NUMBER 为分区数。和消费者的数量有关-->
<!--TOPIC_NAME 为topic的名称，开发时需要指定对应的topic。-->

partitions 的数量和consumer 的数量有关。1：1

3. 删除kafka的topic

```bash
./bin/kafka-topics.sh --delete --zookeeper ZOOKEEPER_HOST:2181 --topic TOPIC_NAME
```

4. 查看topic详情

```bash
./kafka-topics.sh --describe --zookeeper ZOOKEEPER_HOST:2181 --topic TOPIC_NAME
```

5. 查看kafka消费者 消费消息情况

```bash
./kafka-consumer-groups.sh --describe --bootstrap-server KAFKA_HOST:9092 --group Group_Name
```

6. 终端启动一个消费者

```bash
kafka-console-consumer.sh --bootstrap-server KAFKA_HOST:9092 --topic TOPIC_NAME --from-beginning
```
--from-beginning :从消息的第一条开始进行消费，没有 从新发送的进行消费

7. 终端启动一个生产者
```bash

./kafka-console-producer.sh --broker-list KAFKA_HOST:9092 --topic TOPIC_NAME
```

修改topic的partitions 数量
```bash
kafka-topics.sh --alter --zookeeper ZOOKEEPER_HOST:2181 --partitions 18 --topic TOPIC
```


0x. other 
```properties
#消费者消费时的偏移量，latest 是从最新发送的数据开始进行消费，消费者启动前的数据不进行消费。
#earliest 是从上次消费的偏移量开始进行消费，假如第一次，则从最开始进行消费
auto.offset.reset=earliest/latest

```

**server.properties**

```properties
#当外部服务器调用kafka时，需要设置listeners参数，否则无法使用
listeners=PLAINTEXT://10.10.10.81:9092
```






