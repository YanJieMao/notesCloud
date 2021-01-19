## 登录docker

docker login harbor.hao.com

登录过程会保存有授权的配置文件

查看配置文件

```shell
cat ~/.docker/config.json
```

输出

```json

{
        "auths": {
                "harbor.hao.com": {
                        "auth": "aGFvOjE5OTdAWXVhbg=="
                }
        }
}
```



## 在k8s集群中创建一个Secret，包含harbor授权令牌

```
kubectl create secret docker-registry regcred --docker-server=<harbor.hao.com> --docker-username=<hao> --docker-password=<xxxxx> --docker-email=<xxxx@qq.com>
```

```
kubectl create secret docker-registry regcred \
  --docker-server=<harbor.hao.com> \
  --docker-username=<hao> \
  --docker-password=<xxxx> \
  --docker-email=<xxx@qq.com>
```

如果以上命令报错请尝试，查看help

```
kubectl create secret docker-registry NAME --docker-username=user --docker-password=password
--docker-email=email [--docker-server=string] [--from-literal=key1=value1]
```

