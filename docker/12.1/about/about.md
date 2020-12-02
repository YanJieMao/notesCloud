## 1. What is Docker?

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的[Linux](https://baike.baidu.com/item/Linux)机器或Windows 机器上,也可以实现虚拟化,容器是完全使用沙箱机制,相互之间不会有任何接口。

一个完整的Docker有以下几个部分组成：

1. DockerClient客户端
2. Docker Daemon守护进程
3. Docker Image镜像
4. DockerContainer容器 

## 2. What is a Docker Container?

容器是镜像的运行时实例。正如从虚拟机模板上启动 VM 一样，用户也同样可以从单个镜像上启动一个或多个容器。



## 3. What are Docker Images?

容器映像是一个软件的轻量级独立可执行软件包，包含运行它所需的一切：代码，运行时，系统工具，系统库，设置。不管环境如何，集装箱化软件都可以运行相同的Linux和Windows应用程序。容器将软件与其周围环境隔离开来，例如开发环境和登台环境之间的差异，并有助于减少在同一基础架构上运行不同软件的团队之间的冲突。

![](img/20180420204436122)

## 4. What is Docker Hub?

Docker Hub 存放着 Docker 及其组件的所有资源。Docker Hub 可以帮助你与同事之间协作，并获得功能完整的 Docker。为此，它提供的服务有：

- Docker 镜像主机
- 用户认证
- 自动镜像构建和工作流程工具，如构建触发器和 web hooks
- 整合了 GitHub 和 BitBucket

## 5. Explain Docker Architecture?

Docker 包括三个基本概念:

- **镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
- **容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
- **仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。

## 6. What is a Dockerfile?

Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

## 7. Tell us something about Docker Compose.

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

## 8. What is Docker Swarm?

​	Swarm是Docker公司推出的用来**管理docker集群的平台**，几乎全部用GO语言来完成的开发的，代码开源在https://github.com/docker/swarm， 它是将一群Docker宿主机变成一个单一的虚拟主机，Swarm使用标准的Docker API接口作为其前端的访问入口，换言之，各种形式的DockerClient(compose,docker-py等)均可以直接与Swarm通信，甚至Docker本身都可以很容易的与Swarm集成，这大大方便了用户将原本基于单节点的系统移植到Swarm上，同时Swarm内置了对Docker网络插件的支持，用户也很容易的部署跨主机的容器集群服务。

​	Docker Swarm 和 Docker Compose 一样，都是 Docker 官方容器编排项目，但不同的是，Docker Compose 是一个在单个服务器或主机上创建多个容器的工具，而 Docker Swarm 则可以在多个服务器或主机上创建容器集群服务，对于微服务的部署，显然 Docker Swarm 会更加适合。

## 9. What is a Docker Namespace?

​		Namespace是将内核的全局资源做封装，使得每个Namespace都有一份独立的资源，因此不同的进程在各自的Namespace内对同一种资源的使用不会互相干扰。实际上，Linux内核实现namespace的主要目的就是为了实现轻量级虚拟化（容器）服务。在同一个namespace下的进程可以感知彼此的变化，而对外界的进程一无所知。这样就可以让容器中的进程产生错觉，仿佛自己置身于一个独立的系统环境中，以此达到独立和隔离的目的。

> Linux Namespace的6大类型

| 类型              | 功能说明                           |
| ----------------- | ---------------------------------- |
| Mount Namespace   | 提供磁盘挂载点和文件系统的隔离能力 |
| IPC Namespace     | 提供进程间通信的隔离能力           |
| Network Namespace | 提供网络隔离能力                   |
| UTS Namespace     | 提供主机名隔离能力                 |
| PID Namespace     | 提供进程隔离能力                   |
| User Namespace    | 提供用户隔离能力                   |

## 10. What is the lifecycle of a Docker Container?

![这里写图片描述](img/20171208143125220)