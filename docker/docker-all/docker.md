# docker

## 1.初步入手

### 简介



### centos8安装docker



1.添加阿里源

```
sudo yum install -y yum-utils//添加包管理工具
```

```bash
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

```

2.安装

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
yum install -y containerd.io 
yum install -y docker-ce --nobest
```

3.检查docker版本

```bash
#docker -v
```

4.启动docker

```bash 
sudo systemctl start docker
```

运行helloword

```bash
sudo docker run hello-world
```

开机自启动docker

```bash
sudo systemctl enable docker
```

 

## 2.镜像管理

列出镜像

```bash
docker images
```

构建镜像来自dockerfile

```bash
docker build
```

查看镜像历史

```bash
#参数为镜像id或者tag
docker history dd7265748b5d 
docker history mysql 
```

查看一个或多个镜像的详细信息

```bash
#参数为镜像id或者tag
docker inspect dd7265748b5d
docker inspect mysql
```

拉取镜像

```bash
 docker pull nginx //默认最新
 docker pull nginx:1.8 //指定版本
```

向仓库推送镜像

首先得登录

```bash
docker login
```

然后push

```bash
#仓库名称:tag
docker push test:nginx-hao
```

删除一个或者多个镜像

```bash
#可以通过id或者
docker rmi dd7265748b5d
docker rmi dd7265448b5a dd7265728b5b dd7265748b5c//多个镜像
docker rmi mysql
```

### prune -删除未使用的

删除 所有未被 tag 标记和未被容器使用的镜像：

```
docker image prune
```

删除 所有未被容器使用的镜像：

```
docker image prune -a
```

删除 所有停止运行的容器：

```
docker container prune 
```

删除 所有未被挂载的卷：

```
docker volume prune
```

删除 所有网络：

```
docker network prune
```

 删除 docker 所有资源：

```
docker system prune
```



给镜像打tag

```
docker tag ad77889999d test:hao
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```







### 镜像仓库

- 公有仓库

​     hub.docker.com  阿里云  github.com  quay.io

- 自建仓库

​     registry   harbor

## 3.容器管理



#### 

## 网络管理

### 网络模式

Docker支持4种网络模式

 **bridge**
默认网络，Docker启动后默认创建一个docker0网桥，默认创建的容器也是添加到这个网桥中。
 **host**
容器不会获得一个独立的network namespace，而是与宿主机共用一个。
 **none**
获取独立的network namespace，但不为容器进行任何网络配置。
 **container**
与指定的容器使用同一个network namespace，网卡配置也都是相同的。

## Dockerfile

