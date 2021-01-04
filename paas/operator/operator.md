# operator模式

## 简介

Operator 是 Kubernetes 的扩展软件，它利用 [定制资源](https://kubernetes.io/zh/docs/concepts/extend-kubernetes/api-extension/custom-resources/) 管理应用及其组件。 Operator 遵循 Kubernetes 的理念，特别是在[控制器](https://kubernetes.io/zh/docs/concepts/architecture/controller/) 方面。

官方链接：https://kubernetes.io/zh/docs/concepts/extend-kubernetes/operator/

 Operator 模式会封装编写的（Kubernetes 本身提供功能以外的）任务自动化代码。

## operator示例

使用 Operator 可以自动化的事情包括：

- 按需部署应用
- 获取/还原应用状态的备份
- 处理应用代码的升级以及相关改动。例如，数据库 schema 或额外的配置设置
- 发布一个 service，要求不支持 Kubernetes API 的应用也能发现它
- 模拟整个或部分集群中的故障以测试其稳定性
- 在没有内部成员选举程序的情况下，为分布式应用选择首领角色

**详细示例**

1. 有一个名为 SampleDB 的自定义资源，你可以将其配置到集群中。
2. 一个包含 Operator 控制器部分的 Deployment，用来确保 Pod 处于运行状态。
3. Operator 代码的容器镜像。
4. 控制器代码，负责查询控制平面以找出已配置的 SampleDB 资源。
5. Operator 的核心是告诉 API 服务器，如何使现实与代码里配置的资源匹配。
   - 如果添加新的 SampleDB，Operator 将设置 PersistentVolumeClaims 以提供 持久化的数据库存储，设置 StatefulSet 以运行 SampleDB，并设置 Job 来处理初始配置。
   - 如果你删除它，Operator 将建立快照，然后确保 StatefulSet 和 Volume 已被删除。
6. Operator 也可以管理常规数据库的备份。对于每个 SampleDB 资源，Operator 会确定何时创建（可以连接到数据库并进行备份的）Pod。这些 Pod 将依赖于 ConfigMap 和/或具有数据库连接详细信息和凭据的 Secret。
7. 由于 Operator 旨在为其管理的资源提供强大的自动化功能，因此它还需要一些 额外的支持性代码。在这个示例中，代码将检查数据库是否正运行在旧版本上， 如果是，则创建 Job 对象为你升级数据库。