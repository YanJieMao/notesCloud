# go语言入门

# 1.简介

​	**Go**（又称**Golang**）是[Google](https://baike.baidu.com/item/Google)开发的一种[静态](https://baike.baidu.com/item/静态)[强类型](https://baike.baidu.com/item/强类型)、编译型、并发型，并具有垃圾回收功能的[编程语言](https://baike.baidu.com/item/编程语言)。

​	Go的语法接近[C语言](https://baike.baidu.com/item/C语言)，但对于变量的声明有所不同。Go支持垃圾回收功能。Go的并行模型是以[东尼·霍尔](https://baike.baidu.com/item/东尼·霍尔)的[通信顺序进程](https://baike.baidu.com/item/通信顺序进程)（CSP）为基础，采取类似模型的其他语言包括[Occam](https://baike.baidu.com/item/Occam)和[Limbo](https://baike.baidu.com/item/Limbo)，但它也具有Pi运算的特征，比如通道传输。在1.8版本中开放插件（Plugin）的支持，这意味着现在能从Go中动态加载部分函数。

​	与C++相比，Go并不包括如[枚举](https://baike.baidu.com/item/枚举)、[异常处理](https://baike.baidu.com/item/异常处理)、[继承](https://baike.baidu.com/item/继承)、[泛型](https://baike.baidu.com/item/泛型)、[断言](https://baike.baidu.com/item/断言)、[虚函数](https://baike.baidu.com/item/虚函数)等功能，但增加了 切片(Slice) 型、并发、管道、垃圾回收、接口（Interface）等特性的语言级支持。Go 2.0版本将支持泛型，对于断言的存在，则持负面态度，同时也为自己不提供类型继承来辩护。

​	不同于[Java](https://baike.baidu.com/item/Java)，Go内嵌了[关联数组](https://baike.baidu.com/item/关联数组)（也称为[哈希表](https://baike.baidu.com/item/哈希表)（Hashes）或字典（Dictionaries）），就像字符串类型一样。

## 1.1安装

下载安装go

https://golang.org/doc/install#download

下载完成后双击安装，一切默认就好。

确认已经安装完成

- 打开cmd
- 输入go version

控制台输出 例如 ：go version go1.15.5 windows/amd64 ，就是安装成功了

## 1.2第一个go程序hello

1.找到一个喜欢的目录，新建文件夹hello

2.新建一个hello.go 的文件

3.在文本编辑器中编辑hello.go

```go
package main

import "fmt"

func main(){
	fmt.Println("hello,go")
}
```

4.在cmd中找到切换到hello.go所在的目录，输入go run hello.go,控制台输出 hello,go ，我们的第一个程序就完成了。

## 1.3配置vscode开发环境

环境变量配置

系统变量 path  设置   C:\Go\bin

### 1.设置GOPATH路径（GOPATH路径是我们的工作区）

```bash
go env -w GOPATH=我们自己的工作区路径
例如我的就设为 /Users/hao/go
```

### 2.先打开GoMOD，再配置代理

```bash
$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,http://172.26.1.9:5000,direct
```

之后就可以打开VsCode

### 3.在vscode中安装GO插件

进入Extensions后直接搜索go，即可安装

![img](img/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNTA3MjQ5OS0xMTI3ZDhhMmU1Njk4MWI3LnBuZw)

### 4.新建hello.go文件，并用vscode打开。

在安装了Go插件后的VsCode，现在打开go文件后，会自动安装我们自己的必要的环境依赖

支持多条件查询微服务应用