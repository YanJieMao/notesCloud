# jaejer

# 1.初识jaejer

## 1.1jaejer简介

Uber开发的一个受Dapper和Zipkin启发的分布式跟踪系统.后端用Go实现，前端用React实现。

官方地址https://www.jaegertracing.io/

适用于以下场景:

- 分布式跟踪信息传递
- 分布式事务监控
- 服务依赖性分析
- 展示跨进程调用链
- 定位问题
- 性能优化

### 组件

![jaeger-architecture.png](img/jaeger-architecture.png)

Jaeger包含以下主要组件：

- 客户端库`jaeger-client-*`：支持多种语言的客户端库，如Go, Java, Python等语言
- 客户端代理`jaeger-agent`：客户端代理负责将追踪数据转发到服务端，这样能方便应用的快速处理，同时减轻服务端的直接压力；另外可以在客户端代理动态调整采样的频率，进行追踪数据采样的控制
- 数据收集器`jaeger-collector`：主要进行数据收集和处理，从客户端代理收集数据进行处理后持久化到数据存储中
- 数据存储：目前Jaeger支持将收集到的数据持久化到`Cassandra`或`Elasticsearch`里面
- 数据查询`jaeger-query`：主要根据不同的条件到数据存储中进行搜索，支撑前端页面的展示
- `jaeger-ui`：是一个基于React的前端应用，作为jaeger的webui
- `jaeger spark`: 是一个基于spark的后处理和聚合数据管道，可以完成jaeger的服务依赖分析