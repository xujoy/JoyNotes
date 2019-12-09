



docker 私有仓库构建



1.搭建docker 环境

2.拉取registry 镜像文件

```shell
 docker pull registry
```



3.创建 registry 容器

```shell
docker run -d -p 5000:5000 -v /opt/registry:/var/lib/registry --restart=always --name registry registr
```

​	run -d   后台运行

​	-p  映射的端口号

​	-v  映射的文件 目录

​	--restart=always 是否自动重启

​	--name 容器名称

4.验证容器是否启动成功



浏览器打开：http://10.10.11.3:5000/v2/_catalog

或者shell

```shell
curl http://10.10.11.3:5000/v2/_catalog
```



成功则返回：

```json
{"repositories":[]}

```

​	

5.修改/etc/docker/daemon.json文件

```json
{
  "insecure-registries":["10.10.11.3:5000"],
  }

```

10.10.11.3 更改为自己的ip

或者修改docker 的启动配置文件  /etc/sysconfig/docker

```
vim /etc/sysconfig/docker
```

向OPTIONS 里面添加 --insecure-registry 10.10.11.3:5000

```
OPTIONS='--insecure-registry 10.10.11.3:5000 --selinux-enabled --log-driver=journald --signature-verification=false'
if [ -z "${DOCKER_CERT_PATH}" ]; then
    DOCKER_CERT_PATH=/etc/docker
fi
```

修改daemon.json 可能导致 docker 服务无法重启成功，是因为两个配置文件发生了冲突。如果发生冲突删除/etc/sysconfig/docker 文件里  OPTIONS 里面的 --log-driver=journald

6.重启docker

```
systemctl restart docker 
```



7.创建镜像tag



```shell
docker tag 镜像 10.10.11.3:5000/镜像
```



8.push 镜像

```
docker push 10.10.11.3:5000/镜像
```



9.查看是否push 成功



```
curl http://10.10.11.3:5000/v2/_catalog
```



返回：



```
{"repositories":["evileyetool1"]}
```

































