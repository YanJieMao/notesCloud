## 第一个cobra程序

### 起步

1.新建文件夹test

2.cobra init --pkg-name test

3.然后cobra就会创建一系列文件

## cobra-add

这个命令用来创建子命令，子命令就是像下面这样：

- app serve
- app config
- app config create

在你项目的目录下，运行下面这些命令：

```bash
[root@dev-intern-1 first]# cobra add serve
serve created at /root/go/src/base/Iris-learn/cobra/first
[root@dev-intern-1 first]# cobra add config
config created at /root/go/src/base/Iris-learn/cobra/first
[root@dev-intern-1 first]# cobra add create -p 'configCmd'
create created at /root/go/src/base/Iris-learn/cobra/first

```

## 运行程序

首先 go mod init test  go mod tidy 来导入需要的依赖

然后 go run main.go 程序就跑起来了 运行结果如下

```bash
[root@dev-intern-1 test]# go run main.go
A longer description that spans multiple lines and likely contains
examples and usage of using your application. For example:

Cobra is a CLI library for Go that empowers applications.
This application is a tool to generate the needed files
to quickly create a Cobra application.

Usage:
  test [command]

Available Commands:
  config      A brief description of your command
  help        Help about any command
  serve       A brief description of your command

Flags:
      --config string   config file (default is $HOME/.test.yaml)
  -h, --help            help for test
  -t, --toggle          Help message for toggle

Use "test [command] --help" for more information about a command.

```

