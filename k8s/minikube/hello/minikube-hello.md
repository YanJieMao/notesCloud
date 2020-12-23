## minukube-hello

### 启动

通过以下命令启动（前提是已经安装好了minikube）

```shell
minikube start 
```

启动dashboard

```bash
minikube dashboard
```

然后使用浏览器访问

### 创建 Deployment

1.使用 `kubectl create` 命令创建管理 Pod 的 Deployment。该 Pod 根据提供的 Docker 镜像运行 Container

```bash
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
```

2.查看Deployment

```
[x@dev-intern-1 ~]$ kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   0/1     1            0           94s
```

3.查看pod:

```
[x@dev-intern-1 ~]$ kubectl get pods
NAME                          READY   STATUS         RESTARTS   AGE
hello-node-7567d9fdc9-8k6cg   0/1     ErrImagePull   0          2m53s

```

4.查看集群时间

```bash
kubectl get events
```

5.查看集群配置

```
kubectl config view
```

### 创建service

默认情况下，Pod 只能通过 Kubernetes 集群中的内部 IP 地址访问。 要使得 hello-node 容器可以从 Kubernetes 虚拟网络的外部访问，你必须将 Pod 暴露为 Kubernetes Service。

1.使用 `kubectl expose` 命令将 Pod 暴露给公网：

```bash
[x@dev-intern-1 ~]$ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
service/hello-node exposed
```

这里的 `--type=LoadBalancer` 标志表明你希望将你的 Service 暴露到集群外部。

2.查看刚刚创建的service

```bash
[x@dev-intern-1 ~]$ kubectl get services
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
hello-node   LoadBalancer   10.111.167.243   <pending>     8080:31137/TCP   45s
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP          21h

```

对于支持负载均衡器的云服务平台而言，平台将提供一个外部 IP 来访问该服务。 在 Minikube 上，`LoadBalancer` 使得服务可以通过命令 `minikube service` 访问。

3.运行命令让hello跑起来

```
minikube service hello-node
```

### 启用插件

1.列出可用的插件

```
minikube addons list
```

结果如下

```
|-----------------------------|----------|--------------|
|         ADDON NAME          | PROFILE  |    STATUS    |
|-----------------------------|----------|--------------|
| ambassador                  | minikube | disabled     |
| csi-hostpath-driver         | minikube | disabled     |
| dashboard                   | minikube | enabled ✅   |
| default-storageclass        | minikube | enabled ✅   |
| efk                         | minikube | disabled     |
| freshpod                    | minikube | disabled     |
| gcp-auth                    | minikube | disabled     |
| gvisor                      | minikube | disabled     |
| helm-tiller                 | minikube | disabled     |
| ingress                     | minikube | disabled     |
| ingress-dns                 | minikube | disabled     |
| istio                       | minikube | disabled     |
| istio-provisioner           | minikube | disabled     |
| kubevirt                    | minikube | disabled     |
| logviewer                   | minikube | disabled     |
| metallb                     | minikube | disabled     |
| metrics-server              | minikube | disabled     |
| nvidia-driver-installer     | minikube | disabled     |
| nvidia-gpu-device-plugin    | minikube | disabled     |
| olm                         | minikube | disabled     |
| pod-security-policy         | minikube | disabled     |
| registry                    | minikube | disabled     |
| registry-aliases            | minikube | disabled     |
| registry-creds              | minikube | disabled     |
| storage-provisioner         | minikube | enabled ✅   |
| storage-provisioner-gluster | minikube | disabled     |
| volumesnapshots             | minikube | disabled     |
|-----------------------------|----------|--------------|

```

2.启用插件，metrics-server:

```bash
[x@dev-intern-1 ~]$ minikube addons enable metrics-server
* The 'metrics-server' addon is enabled

```

3.查看创建的pod

```bash
kubectl get pod,svc -n kube-system
```

结果如下

```
[x@dev-intern-1 ~]$ kubectl get pod,svc -n kube-system
NAME                                   READY   STATUS    RESTARTS   AGE
pod/coredns-54d67798b7-m7x28           1/1     Running   0          21h
pod/etcd-minikube                      1/1     Running   0          21h
pod/kube-apiserver-minikube            1/1     Running   0          21h
pod/kube-controller-manager-minikube   1/1     Running   0          21h
pod/kube-proxy-glkrq                   1/1     Running   0          21h
pod/kube-scheduler-minikube            1/1     Running   0          21h
pod/metrics-server-868987f44f-ctvs8    1/1     Running   0          28s
pod/storage-provisioner                1/1     Running   0          21h

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
service/kube-dns         ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP   21h
service/metrics-server   ClusterIP   10.105.233.104   <none>        443/TCP                  28s

```

4.禁用插件

```
minikube addons disable metrics-server
```

```bash
[x@dev-intern-1 ~]$ minikube addons disable metrics-server
* "The 'metrics-server' addon is disabled
```

### 清理

清理集群中创建的资源

```bash
[x@dev-intern-1 ~]$ kubectl delete service hello-node 
service "hello-node" deleted
[x@dev-intern-1 ~]$ kubectl delete deployment hello-node
deployment.apps "hello-node" deleted

```

停止minikube虚拟机

```
minikube stop
```



删除minikube虚拟机

```
minikube delete
```

