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
$ go env -w GOPROXY=https://goproxy.cn,direct
```

之后就可以打开VsCode

### 3.在vscode中安装GO插件

进入Extensions后直接搜索go，即可安装

![img](img/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNTA3MjQ5OS0xMTI3ZDhhMmU1Njk4MWI3LnBuZw)

### 4.新建hello.go文件，并用vscode打开。

在安装了Go插件后的VsCode，现在打开go文件后，会自动安装我们自己的必要的环境依赖



>  ps: 特别注意 go mod init xxxx(工程名称)  



# 2.入门

## 2.1命令行参数

`os`包以跨平台的方式，提供了一些与操作系统交互的函数和变量。程序的命令行参数可从os包的Args变量获取；os包外部使用os.Args访问该变量。

os.Args变量是一个字符串（string）的*切片*（slice）（切片就是一个简版的动态数组）

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	var s, sep string
	s = "123"

	for i := 1; i < len(os.Args); i++ {
		s += sep + os.Args[i]
		sep = "ABC"
	}

	fmt.Println(s)
}

```

## 2.1查找重复的行

对文件做拷贝、打印、搜索、排序、统计或类似事情的程序都有一个差不多的程序结构：一个处理输入的循环，在每个元素上执行计算处理，在处理的同时或最后产生输出。我们会展示一个名为`dup`的程序的三个版本；灵感来自于Unix的`uniq`命令，其寻找相邻的重复行。该程序使用的结构和包是个参考范例，可以方便地修改。

`dup`的第一个版本打印标准输入中多次出现的行，以重复次数开头。该程序将引入`if`语句，`map`数据类型以及`bufio`包。





**。。。。。未完待续**

# 3.程序结构



## 3.1命名

一个名字必须以一个**字母**（Unicode字母）或**下划线**开头，后面可以跟任意数量的字母、数字或下划线。

严格区分大小写

大写字母和小写字母是不同的：heapSort和Heapsort是两个不同的名字。

Go语言中类似if和switch的关键字有25个；关键字不能用于自定义名字，只能在特定语法结构中使用。

```
break      default       func     interface   select
case       defer         go       map         struct
chan       else          goto     package     switch
const      fallthrough   if       range       type
continue   for           import   return      var
```

大约30多个预定义的名字，比如int和true等，主要对应内建的常量、类型和函数。

```
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error

内建函数: make len cap new append copy close delete
          complex real imag
          panic recover
```

ps：这些并不是关键字，可以在定义中重新使用他们，但是避免混乱

**命名的可见范围**

- 如果一个名字是在函数内部定义，那么它就只在函数内部有效。
- 如果是在函数外部定义，那么将在当前包的所有文件中都可以访问。

名字的开头字母的**大小写**决定了名字在包外的可见性。

如果一个名字是大写字母开头的（ps：*必须是在函数外部定义的包级名字；包级函数名本身也是包级名字*），那么它将是导出的，也就是说可以被外部的包访问.

例如fmt包的Printf函数就是导出的，可以在fmt包外部访问。包本身的名字一般总是用小写字母。

名字长短无限制，尽量短小，推荐驼峰式命名法，用大小写分割

## 3.2声明

声明语句定义了对象或对象的部分或全部属性。

四种声明语句：

- var 变量 
- const 常量
- type 类型
- func 函数实体对象

  每个源文件中以包的声明语句开始，说明该源文件是属于哪个包。包声明语句之后是import语句导入依赖的其它包，然后是包一级的类型、变量、常量、函数的声明语句，包一级的各种类型的声明语句的顺序无关紧要.(函数内部的名字则必须先声明之后才能使用)

```go
// Boiling 输出水的沸点
package main

import "fmt"

const boilingF = 212.0  //包一级范围内声明，包级范围

func main() {
	var f = boilingF        //函数内声明，函数级范围
	var c = (f - 32) * 5 / 9  //函数内声明，函数级范围
	fmt.Printf("boiling point = %g°F or %g°C\n", f, c)

}

```

一个函数的声明由一个函数名字、参数列表（由函数的调用者提供参数变量的具体值）、一个可选的返回值列表和包含函数定义的函数体组成。如果函数没有返回值，那么返回值列表是省略的。执行函数从函数的第一个语句开始，依次顺序执行直到遇到return返回语句，如果没有返回语句则是执行到函数末尾，然后返回到函数调用者。

函数调用程序

```go
package main

import "fmt"

func main() {

	const freezingF, boilingF = 32.0, 212.0
	fmt.Printf("%g°F = %g°C\n", freezingF, fToC(freezingF))
	fmt.Printf("%g°F = %g°C\n", freezingF, fToC(boilingF))
}

func fToC(f float64) float64 {
	return (f - 32) * 5 / 9
}

```

## 3.3变量

变量的一般声明语法

```
var 变量名称 类型 = 表达式
例如：var hao int = 123

```

“*类型*”或“*= 表达式*”两个部分可以省略其中的一个。如果省略的是类型信息，那么将根据初始化表达式来推导变量的类型信息。如果初始化表达式被省略，那么将用零值初始化该变量。

- 数值型 0值
- 布尔类型 false
- 字符串  空字符
- 接口或引用类型（包括slice、指针、map、chan和函数）变量对应的零值是nil
- 数组或结构体等聚合类型对应的零值是每个元素或字段都是对应该类型的零值。

**零值初始化机制优点**

可以确保每个声明的变量总是有一个良好定义的值，因此在Go语言中不存在未初始化的变量。

可以简化很多代码，而且可以在没有增加额外工作的前提下确保边界条件下的合理行为。

例

```go
var s string
fmt.Println(s) // ""
```

```go
var i, j, k int                 // int, int, int
var b, f, s = true, 2.3, "four" // bool, float64, string
```

初始化表达式可以是字面量或任意的表达式。

**初始化的时间**

包级别声明的变量会在main入口函数执行前完成初始化，

局部变量将在声明语句被执行到的时候完成初始化。

ps:一组变量也可以通过调用一个函数，由函数返回的多个返回值初始化

```go
var f, err = os.Open(name) // os.Open returns a file and an error
```

