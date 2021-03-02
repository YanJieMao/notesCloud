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

### 3.3.1简短变量声明

在函数内部，以“名字 := 表达式”形式声明变量，变量的类型根据表达式来自动推导。

例如

```go
anim := gif.GIF{LoopCount: nframes}
freq := rand.Float64() * 3.0
t := 0.0
```

简洁，灵活，简短变量声明被广泛用于大部分的局部变量的声明和初始化。

var形式的声明语句往往是用于需要**显式指定变量类型**的地方

或者因为变量稍后会被重新赋值而初始值**无关紧要**的地方。

```
i := 100                  // an int
var boiling float64 = 100 // a float64
var names []string
var err error
var p Point
```

简短变量声明初始化**一组变量**

```go
i, j := 0, 1
```

> 注意： 请记住“:=”是一个变量声明语句，而“=”是一个变量赋值操作。也不要混淆多个变量的声明和元组的多重赋值，后者是将右边各个表达式的值赋值给左边对应位置的各个变量：

```go
i, j = j, i // 交换 i 和 j 的值 （多重赋值）
```

简短变量声明语句也可以用函数的返回值来声明和初始化变量

```go
f, err := os.Open(name)
if err != nil {
    return err
}
// ...use f...
f.Close()
```

ps: 如果有一些已经在相同的词法域声明过了，那么简短变量声明语句对这些已经声明过的变量就只有赋值行为.

简短变量声明语句中必须至少要声明一个新的变量，

```go
f, err := os.Open(infile)
// ... 编译不能通过
f, err := os.Create(outfile) // compile error: no new variables
// 解决的方法是第二个简短变量声明语句改用普通的多重赋值语句。
```

 简短变量声明语句只有对已经在同级词法域声明过的变量才和赋值操作语句等价，如果变量是在外部词法域声明的，那么简短变量声明语句将会在当前词法域重新声明一个新的变量。

### 3.3.2指针

一个变量对应一个保存了变量对应类型值的内存空间。普通变量在声明语句创建时被绑定到一个变量名

一个指针的值是另一个变量的地址。一个指针对应变量在内存中的存储位置。

并不是每一个值都会有一个内存地址，但是对于每一个变量必然有对应的内存地址。

通过指针，我们可以直接读或更新对应变量的值，而不需要知道该变量的名字

**指针->地址** 

“var x int”声明语句声明一个x变量，那么&x表达式（取x变量的内存地址）将产生一个指向该整数变量的指针，指针对应的数据类型是`*int`，指针被称之为“指向int类型的指针”。

```go
x := 1
p := &x         // p, 指向int类型的指针
fmt.Println(*p) // "1"
*p = 2          // 修改值 x = 2
fmt.Println(x)  // "2"
```

任何类型的指针的零值都是nil。指针之间可以相等测试，只有当它们指向同一个变量或全部是nil时才相等。

在Go语言中，返回函数中局部变量的地址也是安全的。

例如

```go
//调用f函数时创建局部变量v，在局部变量地址被返回之后依然有效，因为指针p依然引用这个变量。
var p = f()

func f() *int {
    v := 1
    return &v
}
```

注意：*p++ // 非常重要：只是增加p指向的变量的值，并不改变p指针！！！

指针是实现标准库中**flag包**的关键技术，它使用命令行参数来设置对应变量的值，而这些对应命令行标志参数的变量可能会零散分布在整个程序中。(?什么是flag包)

```go
// Echo4 prints its command-line arguments.
package main

import (
    "flag"
    "fmt"
    "strings"
)

var n = flag.Bool("n", false, "omit trailing newline")
var sep = flag.String("s", " ", "separator")

func main() {
    flag.Parse()
    fmt.Print(strings.Join(flag.Args(), *sep))//sep和n变量分别是指向对应命令行标志参数变量的指针，
    if !*n {									//因此必须用*sep和*n形式的指针语法间接引用它们
        fmt.Println()
    }
}
```

加h参数运行结果

```bash
PS C:\Users\Troila\go\base\ch2\echo4> go run echo4.go -h
Usage of C:\Users\Troila\AppData\Local\Temp\go-build826247146\b001\exe\echo4.exe:
  -n    omit trailing newline
  -s string
        separator (default " ")
```

测试用例

```
$ go build gopl.io/ch2/echo4
$ ./echo4 a bc def
a bc def
$ ./echo4 -s / a bc def
a/bc/def
$ ./echo4 -n a bc def
a bc def$
$ ./echo4 -help
Usage of ./echo4:
  -n    omit trailing newline
  -s string
        separator (default " ")
```

### 3.2.3new函数

另一个创建变量的方法是调用内建的new函数。

表达式new(T)将创建一个T类型的匿名变量，初始化为T类型的零值，然后返回变量地址，返回的指针类型为`*T`

```go
p := new(int)   // p, *int 类型, 指向匿名的 int 变量
fmt.Println(*p) // "0"
*p = 2          // 设置 int 匿名变量的值为 2
fmt.Println(*p) // "2"
```

new创建变量和声明式创建变量没有什么不同（除了不需要名字），这不是一种新的概念，而是一种语法糖

```go
func newInt() *int {
    return new(int)
}

func newInt() *int {
    var dummy int
    return &dummy
}
```

每次调用new函数都是返回一个新的变量的地址，因此下面两个地址是不同的：

```go
p := new(int)
q := new(int)
fmt.Print(q == p)//false
```

new只是一个预定于的函数，并不是一个关键字，在其他函数中我们可以定义 new int ,把new定义为其他类型

### 3.3.4变量的生命周期

- 包一级：生命周期和整个程序的运行周期是一致的
- 局部变量： 从创建一个新变量的声明语句开始，直到该变量不再引用，变量存储空间可能会被回收。
- 函数的参数变量和返回值变量都是局部变量。它们在函数每次被调用的时候创建。

Go自动垃圾收集器何时回收变量？

基本的实现思路是，从每个包级的变量和每个当前运行函数的每一个局部变量开始，通过指针或引用的访问路径遍历，是否可以找到该变量。如果不存在这样的访问路径，那么说明该变量是不可达的，也就是说它是否存在并不会影响程序后续的计算结果。

```go
var global *int

func f() {
    var x int
    x = 1
    global = &x //变量x在函数结束后不可以回收,global引用
}

func g() {
    y := new(int)
    *y = 1  //y可以回收
}
```

## 3.4赋值



```Go
x = 1                       // 命名变量的赋值
*p = true                   // 通过指针间接赋值
person.name = "bob"         // 结构体字段赋值
count[x] = count[x] * scale // 数组、slice或map的元素赋值
count[x] *= scale //简洁形式

```

赋值支持++自增和--自减语句

```go
v := 1
v++    // 等价方式 v = v + 1；v 变成 2
v--    // 等价方式 v = v - 1；v 变成 1
```

ps :需要注意 x++ 是语句不是表达式，x = x++是错误的:x:

### 3.4.1元组赋值

元组赋值是另一种形式的赋值语句，它允许同时更新多个变量的值。

赋值之前有变表达式先计算，然后再进行赋值

```go
x, y = y, x

a[i], a[j] = a[j], a[i]
```

元组赋值使一系列琐碎赋值更紧凑，特别是for循环。

有些表达式会产生多个值，比如调用一个有多个返回值的函数。当这样一个函数调用出现在元组赋值右边的表达式中时（右边不能再有其它表达式），左边变量的数目必须和右边一致。

```
f, err = os.Open("foo.txt") // function call returns two values
```

```
v, ok = m[key]             // map lookup
v, ok = x.(T)              // type assertion
v, ok = <-ch               // channel receive
```

```
v = m[key]                // map查找，失败时返回零值
v = x.(T)                 // type断言，失败时panic异常
v = <-ch                  // 管道接收，失败时返回零值（阻塞不算是失败）

_, ok = m[key]            // map返回2个值
_, ok = mm[""], false     // map返回1个值
_ = mm[""]                // map返回1个值
```

和变量声明一样，我们可以用下划线空白标识符`_`来丢弃不需要的值。

```
_, err = io.Copy(dst, src) // 丢弃字节数
_, ok = x.(T)              // 只检测类型，忽略具体值
```

### 2.4.2可赋值性

赋值语句是显式的赋值形式，

隐式的赋值行为：

- 函数调用会隐式地将调用参数的值赋值给函数的参数变量，
- 一个返回语句会隐式地将返回操作的值赋值给结果变量，
- 一个复合类型的字面量也会产生赋值行为。

```go
medals := []string{"gold", "silver", "bronze"}
```

只有右边的值对于左边的变量是可赋值的，赋值语句才是允许的。

