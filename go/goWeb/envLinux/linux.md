

## linux配置go环境

```
[root@kiccleaf ~]# wget https://golang.google.cn/dl/go1.15.linux-amd64.tar.gz
[root@kiccleaf ~]# tar zxvf go1.15.linux-amd64.tar.gz
[root@kiccleaf ~]# mv go /usr/local/
[root@kiccleaf ~]# vim /etc/profile
在文件底部增加：
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
保存退出后
[root@kiccleaf ~]# source /etc/profile

```

## 测试

```
[root@kiccleaf ~]# go version
go version go1.15 linux/amd64

[root@kiccleaf ~]# vim helloword.go
```

