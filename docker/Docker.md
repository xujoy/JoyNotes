

Docker 安装

```
yum install -y docker 
```

启动

```
 systemctl start docker
```







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



#### 镜像名称修改

```bash
docker tag 镜像名称:版本 新镜像名称:新版本号
```



#### 镜像导出

```bash
docker save > 文件名称.tar 镜像名称:版本号
```

####镜像导入

```bash
docker load < 文件名称.tar
```




