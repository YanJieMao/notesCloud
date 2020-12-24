## 修改host

在host添加如下

vim /etc/hosts

ip地址 和主机名称 修改为自己设置的

```
127.0.0.1 harbor.hao.com
```

## 登录

```
docker login harbor.hao.com
```

ps:如果出现以下报错

```
Error response from daemon: Get https://harbor.hao.com/v2/: dial tcp 52.128.23.153:443: connect: connection refused
```

是因为证书验证不通过

在/etc/docker/certs.d/目录下

新建harbor.hao.com目录

然后把安装harbor生成的证书复制到harbor.hao.com下，修改文件名称为ca,后缀名不变

然后重新启动docker服务systemctl restart docker.service