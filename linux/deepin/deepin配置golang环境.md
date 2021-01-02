安装1.5.3

   下载

```
wget https://storage.googleapis.com/golang/go1.15.3.linux-amd64.tar.gz
```

   解压 

```
tar zxvf go1.15.3.linux-amd64.tar.gz
```



  移到/usr/local/go

```
mv go /usr/local/go
```



   **配置go环境**

```
vi /etc/profile
#Go Configuration
export GOROOT=/usr/local/go
这是你Go的安装目录
export GOARCH=amd64
这是你的处理器类型……例如i386，amd64
export GOOS=linux
这是你的操作系统类型。
export GOPATH=/home/hao/code/go
这是你的 Go 项目文件夹，如果我们采用已经编译完成的软件包，就不需要这个
export PATH=$GOROOT/bin:$PATH
你的Go命令文件夹，请保持默认……
```



   生效环境变量

```
source /etc/profile
```

运行

```
go version
```

可看到go版本 则安装成功