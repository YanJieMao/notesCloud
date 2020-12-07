## 先编写一个httpServer

```go
package main

import (
        "fmt"
        "net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {

        fmt.Fprintf(w, "hello\n")
}

func sum(w http.ResponseWriter, req *http.Request) {

        fmt.Fprintf(w, "sum\n")
}

func headers(w http.ResponseWriter, req *http.Request) {

        for name, headers := range req.Header {
                for _, h := range headers {
                        fmt.Fprintf(w, "%v: %v\n", name, h)
                }
        }
}

func main() {

        http.HandleFunc("/hello", hello)
        http.HandleFunc("/headers", headers)
        http.HandleFunc("/sum", sum)

        http.ListenAndServe(":8080", nil)
}

```

## 在工程文件下创建Dockerfile

dockerfile

```bash
#源镜像
FROM golang:alpine
#作者
MAINTAINER mkdirhao "mkdirhao@qq.com"
#设置工作目录
WORKDIR $GOPATH/src/goWeb/12.7/http_server
#将服务器的go工程代码加入到docker容器中
ADD . $GOPATH/src/goWeb/12.7/http_server
#代理
ENV GO111MODLE=ON
ENV GOPROXY="https://groproxy.io"
#go构建可执行文件
RUN go build .
#暴露端口
EXPOSE 8080
#最终运行docker的命令
ENTRYPOINT  ["./http_server"]

```

## 构建镜像

在工程文件下运行

```bash
docker build -t http_server .
```

## 运行镜像



```bash
docker run --name http_server -p 8080:8080 -d http_server
```

## 测试

浏览器访问 http://192.168.56.102:8080/hello （就是相应的地址）

浏览器中显示hello