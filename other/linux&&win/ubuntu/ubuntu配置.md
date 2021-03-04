## ubuntu 更换源

1.备份

```bash
sudo cp -v /etc/apt/source.list /etc/apt/source.list.backup
```

执行chmod命令更改文件权限使软件源文件可编辑：

```
sudo chmod 777 /etc/apt/sources.list
```


之后通过gedit命令编辑软件源：

```
sudo gedit /etc/apt/sources.list
```


如果报错提示：

```
sudo: gedit: command not found
```


那么直接使用vim修改：

```
vim /etc/apt/sources.list
```

阿里源

```bash
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

## ubuntu20.04配置golang

1.下载并解压

```bash
wget -c https://dl.google.com/go/go1.15.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local

```

2 调整环境变量

通过将 Go 目录添加到`$PATH`环境变量，系统将会知道在哪里可以找到 Go 可执行文件。

这个可以通过添加下面的行到`/etc/profile`文件（系统范围内安装）或者`$HOME/.profile`文件（当前用户安装）：

```text
export PATH=$PATH:/usr/local/go/bin
```

保存文件，并且重新加载新的PATH 环境变量到当前的 shell 会话：

```text
source ~/.profile
```

3 验证 Go 安装过程

通过打印 Go 版本号，验证安装过程。

```text
go version
```

输出应该像下面这样：

```text
go version go1.15 linux/amd64
```