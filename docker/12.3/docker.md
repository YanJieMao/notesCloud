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

 