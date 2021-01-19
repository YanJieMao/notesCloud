## 部署redis

1.在本地写好yaml文件

2.使用kubectl命令行发送给Apiserver

3.Apiserver接收到请求将内容存储到etcd中

4.Controller组件(包括scheduler、replication、endpoint)监控资源变化并作出反应。

5.Scheduler检查数据库变化，将pod分配到可以运行它们的节点上，并更新数据库，记录pod分配情况。

6.Kubelet监控数据库变化，运行pods 启动container

7.指定应用的外部访问方式为NodePort，生成相应的一组转发规则

## 访问redis

用户通过访问相应端口，kube-proxy转发相应的请求到相应的一组pods



