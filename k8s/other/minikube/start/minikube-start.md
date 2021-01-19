# minikube-first

## 简介

minikube 是一个本地的k8s环境，致力于让学习和使用k8s开发变得简单。仅仅需要一个docker容器或者虚拟机环境。

## 安装

centos 7

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
sudo rpm -ivh minikube-latest.x86_64.rpm
```

其他系统参考https://minikube.sigs.k8s.io/docs/start/

## 运行

用docker作为驱动运行，前提得安装docker

 minikube start --driver=docker 

选用阿里云镜像

 minikube start --image-mirror-country cn  

## 与集群进行交互

### kubectl  安装

配置阿里云k8s仓库

vim /etc/yum.repos.d/kubernetes.repo

```
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
```

然后安装 yum install -y kubectl

kubectl

```shell
minikube kubectl -- get po -A
```

dashboard

```shell
minikube dashboard
```

## 部署应用

部署hello，暴露8080端口

```shell
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

run hello

```
kubectl get services hello-minikube
```

```
//从浏览器打开
minikube service hello-minikube
```

## 管理集群

暂停

```shell
minikube pause 
```

停止

```
minikube stop
```

设置内存

```shell
minikube config set memory 4096 //4g内存
```

浏览k8s组件

```shell
minikube addons list
```

跑老版本k8s

```shell
minikube start -p aged --kubernetes-version=v1.16.1
```

删除所有集群

```shell
minikube delete --all
```