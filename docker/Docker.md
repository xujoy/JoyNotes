#### Docker 显示镜像Dockerfile构建详细信息

```
docker history ubuntu:latest --no-trunc
```

#### 删除所有容器（已停止运行的）

```
 docker container prune
```

#### 已有容器创建为新的镜像

```bash
docker commit CONTAINER_NAME NEW_IMAGE_NAME
```
#### docker 构建mysql

```bash
docker run -p 3306:3306 --name mysql_container -e MYSQL_ROOT_PASSWORD=root -d mysql
```