## contex简介

Context通常被译作上下文，它是一个比较抽象的概念。一般理解为程序单元的一个运行状态、现场，上下上下则是存在上下层的传递，上会把内容传递给下。在Go语言中，程序单元也就指的是Goroutine。

每个Goroutine在执行之前，都要先知道程序当前的执行状态，通常将这些执行状态封装在一个Context变量中，传递给要执行的Goroutine中。上下文则几乎已经成为传递与请求同生存周期变量的标准方法。在网络编程下，当接收到一个网络请求Request，处理Request时，我们可能需要开启不同的Goroutine来获取数据与逻辑处理，即一个请求Request，会在多个Goroutine中处理。而这些Goroutine可能需要共享Request的一些信息；同时当Request被取消或者超时的时候，所有从这个Request创建的所有Goroutine也应该被结束。

## contex包

context包的核心就是Context接口，其定义如下：

context包不仅实现了在程序单元之间共享状态变量的方法，同时能通过简单的方法，使我们在被调用程序单元的外部，通过设置ctx变量值，将过期或撤销这些信号传递给被调用的程序单元。在网络编程中，若存在A调用B的API, B再调用C的API，若A调用B取消，那也要取消B调用C，通过在A,B,C的API调用之间传递Context，以及判断其状态，就能解决此问题。

```go
type Context interface {
	Deadline() (deadline time.Time, ok bool)
	Err() error
	Done() <-chan struct{}

	
	Value(key interface{}) interface{}
}
```

Deadline会返回一个超时时间，Goroutine获得了超时时间后，例如可以对某些io操作设定超时时间。

Done方法返回一个信道（channel），当Context被撤销或过期时，该信道是关闭的，即它是一个表示Context是否已关闭的信号。

当Done信道关闭后，Err方法表明Context被撤的原因。

Value可以让Goroutine共享一些数据，当然获得数据是协程安全的。但使用这些数据的时候要注意同步，比如返回了一个map，而这个map的读写则要加锁。

Context接口没有提供方法来设置其值和过期时间，也没有提供方法直接将其自身撤销。也就是说，Context不能改变和撤销其自身。那么该怎么通过Context传递改变后的状态呢？

