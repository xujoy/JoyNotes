##### docker 构建kafka 环境

1. 拉取kafka镜像文件

   ```shell
   docker pull wurstmeister/kafka
   ```
   
2. 拉取zookeeper镜像文件

   ```shell
   docker pull wurstmeister/zookeeper
   ```

####使用docker-compose 构建
1. 创建 docker-compose.yml 文件
```yaml
version: '2'

services:
  zookeeper:
    image: wurstmeister/zookeeper
    expose:
    - "2181"
  kafka:
    image: wurstmeister/kafka
    depends_on:
    - zookeeper 
    ports:
    - "9092:9092"
    environment:
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181

```

Image:所使用的镜像（如果镜像不存在，进行pull，默认latest 版本）

expose：声明的端口号

depends_on:容器的依赖，此为先启动zookeeper

ports：暴露端口信息，宿主端口和容器端口的映射

enviroment:容器所依赖的环境变量

2. 启动 docker-compose up





---


