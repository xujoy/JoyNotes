##### docker 构建kafka 环境

1. 拉取kafka镜像文件

   ```shell
   docker pull wurstmeister/kafka
   ```
   
2. 拉取zookeeper镜像文件

   ```shell
   docker pull wurstmeister/zookeeper
   ```

##### 使用docker-compose 构建
1. 创建 docker-compose.yml 文件

```yaml
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    image: wurstmeister/kafka:2.11-0.11.0.3
    ports:
      - "9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 10.10.10.21
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /opt/joy/mykafka:/kafka
     
```


  Image:所使用的镜像（如果镜像不存在，进行pull，默认latest 版本）
  expose：声明的端口号
  depends_on:容器的依赖，此为先启动zookeeper
  ports：暴露端口信息，宿主端口和容器端口的映射
  enviroment:容器所依赖的环境变量
  volumes: 挂载数据卷(该属性可以在容器中，执行docker命令)
  ****
  10.10.10.21 为宿主机的ip地址
  ****

#### 启动 

```shell
docker-compose up -d
```
 -d ：后台运行

####  扩展broker

```shell
docker-compose scale kafka=4
```


docker ps 查看，kafka构建了4个容器，详看 docker-compose命令


####  查看topic

```
docker exec mykafka_kafka_1 kafka-topics.sh --list --zookeeper zookeeper:2181
```
mykafka_kafka_1 : 是容器的名称(mykafka 是docker-compose.yml存放的文件名称,kafka镜像名称，1是第几个容器，可使用docker ps 查看) 

####  创建kafka 的topic

```shell
docker exec mykafka_kafka_1 kafka-topics.sh --create --topic moac --partitions 4 --zookeeper zookeeper:2181 --replication-factor 2
```
创建topic 名称是moac，4个分区，2个副本。

#### 查看topic 详情

```shell
docker exec mykafka_kafka_1 kafka-topics.sh --describe --topic moac --zookeeper zookeeper:2181
```

```bash
	Topic:moac	PartitionCount:4	ReplicationFactor:2	Configs:
	Topic: moac	Partition: 0	Leader: 1004	Replicas: 1004,1001	Isr: 1004,1001
	Topic: moac	Partition: 1	Leader: 1001	Replicas: 1001,1002	Isr: 1001,1002
	Topic: moac	Partition: 2	Leader: 1002	Replicas: 1002,1003	Isr: 1002,1003
	Topic: moac	Partition: 3	Leader: 1003	Replicas: 1003,1004	Isr: 1003,1004
```
#### 查看kafka宿主机映射的端口号
```bash
[root@evileye-02 mykafka]# docker ps --format "{{.Names}} \t\t\t{{.Ports}}"
mykafka_kafka_1 			0.0.0.0:32792->9092/tcp
mykafka_kafka_3 			0.0.0.0:32794->9092/tcp
mykafka_zookeeper_1 			22/tcp, 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp
mykafka_kafka_2 			0.0.0.0:32793->9092/tcp

```
宿主机的32792映射到 mykafka_kafka_1 容器的9092 端口上。 

宿主机的32793映射到 mykafka_kafka_2 容器的9092 端口上。

宿主机的32794映射到 mykafka_kafka_3 容器的9092 端口上。

#### 启动生产者

1.进入容器   
	
```shell
docker exec -it mykafka_kafka_1 bash
```
	
2.启动生产者
	
```shell
kafka-console-producer.sh --broker-list 10.10.10.21:32792 --topic moac
```
	
#### 启动消费者

打开一个新的终端窗口

1.进入容器   
	
```shell
	docker exec -it mykafka_kafka_1 bash
```	
	
2.启动消费
	
```shell
kafka-console-consumer.sh --zookeeper 10.10.10.21:2181 --topic moac --from-beginning
```
	
	




















---


