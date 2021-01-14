# kubebuilder

# 1.初入门

## 1.1简介

Kubebuilder是一个使用CRD构建Kubernetes API的框架。

[Kubebuilder](https://link.zhihu.com/?target=https%3A//yq.aliyun.com/go/articleRenderRedirect%3Furl%3Dhttps%3A%2F%2Fgithub.com%2Fkubernetes-sigs%2Fkubebuilder) 是一个使用 CRDs 构建 K8s API 的 SDK，主要是：

- 提供脚手架工具初始化 CRDs 工程，自动生成 boilerplate 代码和配置；
- 提供代码库封装底层的 K8s go-client；

方便用户从零开始开发 CRDs，Controllers 和 Admission Webhooks 来扩展 K8s。

## 1.2安装kubebuilder

安装 [kubebuilder](https://sigs.k8s.io/kubebuilder):

```bash
os=$(go env GOOS)
arch=$(go env GOARCH)

# 下载 kubebuilder 并解压到 tmp 目录中
curl -L https://go.kubebuilder.io/dl/2.3.1/${os}/${arch} | tar -xz -C /tmp/
```

If you are using a Kubebuilder plugin version less than version `v3+`, you must configure the Kubernetes binaries required for the [envtest](https://book.kubebuilder.io/reference/testing/envtest.html), run:

```bash
# 将 kubebuilder 移动到一个长期的路径，并将其加入环境变量 path 中
# （如果你把 kubebuilder 放在别的地方，你需要额外设置 KUBEBUILDER_ASSETS 环境变量）

sudo mv /tmp/kubebuilder_2.3.1_${os}_${arch} /usr/local/kubebuilder
export PATH=$PATH:/usr/local/kubebuilder/bin
```

[请使用 master 分支的 kubebuilder 进行构建](https://cloudnative.to/kubebuilder/quick-start.html#请使用-master-分支的-kubebuilder-进行构建)

另外，你可以从 `https://go.kubebuilder.io/dl/latest/${os}/${arch}` 下载安装包

Kubebuilder 通过 `kubebuilder completion <bash|zsh>` 命令为 Bash 和 Zsh 提供了自动完成的支持，这可以节省你大量的重复编码工作。更多信息请参见 [completion](https://cloudnative.to/kubebuilder/reference/completion.html) 文档。

## 1.3新建项目

新建项目文件夹，然后打开文件所在目录，执行init命令

```bash
# 我们将使用 tutorial.kubebuilder.io 域，
# 所以所有的 API 组将是<group>.tutorial.kubebuilder.io.
kubebuilder init --domain tutorial.kubebuilder.io
```

init项目kubebuilder创建的组件

**基础组件**

`go.mod`: 我们的项目的 Go mod 配置文件，记录依赖库信息。

`Makefile`: 用于控制器构建和部署的 Makefile 文件

`PROJECT`: 用于生成组件的 Kubebuilder 元数据

**启动配置**

在 [`config/`](https://github.com/kubernetes-sigs/kubebuilder/tree/master/docs/book/src/cronjob-tutorial/testdata/project/config) 目录下有启动配置。init后只包含了在集群上启动控制器所需的 [Kustomize](https://sigs.k8s.io/kustomize) YAML 定义，后面编写控制器，下面还会有 CustomResourceDefinitions(CRD) 、RBAC 配置和 WebhookConfigurations 。

[`config/default`](https://github.com/kubernetes-sigs/kubebuilder/tree/master/docs/book/src/cronjob-tutorial/testdata/project/config/default) 在标准配置中包含 [Kustomize base](https://github.com/kubernetes-sigs/kubebuilder/blob/master/docs/book/src/cronjob-tutorial/testdata/project/config/default/kustomization.yaml) ，它用于启动控制器。

其他每个目录都包含一个不同的配置，重构为自己的基础。

- [`config/manager`](https://github.com/kubernetes-sigs/kubebuilder/tree/master/docs/book/src/cronjob-tutorial/testdata/project/config/manager): 在集群中以 pod 的形式启动控制器
- [`config/rbac`](https://github.com/kubernetes-sigs/kubebuilder/tree/master/docs/book/src/cronjob-tutorial/testdata/project/config/rbac): 在自己的账户下运行控制器所需的权限

**入口函数**

`main.go`

# 2.构建CronJob

## 2.1入口函数main

我们的 main 文件最开始是 import 一些基本库，尤其是：

- 核心的 [控制器运行时](https://pkg.go.dev/sigs.k8s.io/controller-runtime?tab=doc) 库
- 默认的控制器运行时日志库-- Zap 

```go
package main

import (
	"flag"
	"os"

	"k8s.io/apimachinery/pkg/runtime"
	clientgoscheme "k8s.io/client-go/kubernetes/scheme"
	_ "k8s.io/client-go/plugin/pkg/client/auth/gcp"
	ctrl "sigs.k8s.io/controller-runtime"
	"sigs.k8s.io/controller-runtime/pkg/log/zap"
	// +kubebuilder:scaffold:imports
)
```

每一组控制器都需要一个 [*Scheme*](https://book.kubebuilder.io/cronjob-tutorial/gvks.html#err-but-whats-that-scheme-thing)，它提供了 Kinds 和相应的 Go 类型之间的映射。

```go
var (
	scheme   = runtime.NewScheme()
	setupLog = ctrl.Log.WithName("setup")
)

func init() {
	_ = clientgoscheme.AddToScheme(scheme)

	// +kubebuilder:scaffold:scheme
}
```

main函数代码

```go
func main() {
	var metricsAddr string
	var enableLeaderElection bool
	flag.StringVar(&metricsAddr, "metrics-addr", ":8080", "The address the metric endpoint binds to.")
	flag.BoolVar(&enableLeaderElection, "enable-leader-election", false,
		"Enable leader election for controller manager. "+
			"Enabling this will ensure there is only one active controller manager.")
	flag.Parse()

	ctrl.SetLogger(zap.New(zap.UseDevMode(true)))

	mgr, err := ctrl.NewManager(ctrl.GetConfigOrDie(), ctrl.Options{
		Scheme:             scheme,
		MetricsBindAddress: metricsAddr,
		Port:               9443,
		LeaderElection:     enableLeaderElection,
		LeaderElectionID:   "0f42b276.tutorial.kubebuilder.io",
	})
	if err != nil {
		setupLog.Error(err, "unable to start manager")
		os.Exit(1)
	}

	// +kubebuilder:scaffold:builder

	setupLog.Info("starting manager")
	if err := mgr.Start(ctrl.SetupSignalHandler()); err != nil {
		setupLog.Error(err, "problem running manager")
		os.Exit(1)
	}
}
```

核心逻辑:

- 我们通过 flag 库解析入参
- 我们实例化了一个[*manager*](https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/manager#Manager)，它记录着我们所有控制器的运行情况，以及设置共享缓存和API服务器的客户端（注意，我们把我们的 Scheme 的信息告诉了 manager）。
- 运行 manager，它反过来运行我们所有的控制器和 webhooks。manager 状态被设置为 Running，直到它收到一个优雅停机 (graceful shutdown) 信号。这样一来，当我们在 Kubernetes 上运行时，我们就可以优雅地停止 pod。

> ps：Manager 可以通过以下方式限制控制器可以监听资源的命名空间。

```go
    mgr, err = ctrl.NewManager(ctrl.GetConfigOrDie(), ctrl.Options{
        Scheme:             scheme,
        Namespace:          namespace,
        MetricsBindAddress: metricsAddr,
    })
```

上面的例子将把你的项目改成只监听单一的命名空间。在这种情况下，建议通过将默认的 ClusterRole 和 ClusterRoleBinding 分别替换为 Role 和 RoleBinding 来限制所提供给这个命名空间的授权。

另外，也可以使用 [MultiNamespacedCacheBuilder](https://pkg.go.dev/github.com/kubernetes-sigs/controller-runtime/pkg/cache#MultiNamespacedCacheBuilder) 来监听特定的命名空间。

```go
    var namespaces []string // List of Namespaces

    mgr, err = ctrl.NewManager(ctrl.GetConfigOrDie(), ctrl.Options{
        Scheme:             scheme,
        NewCache:           cache.MultiNamespacedCacheBuilder(namespaces),
        MetricsBindAddress: fmt.Sprintf("%s:%d", metricsHost, metricsPort),
    })
```

更多信息参见 [MultiNamespacedCacheBuilder](https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/cache?tab=doc#MultiNamespacedCacheBuilder)

##   2.2group、version、kind、之间的关系

### GVK 介绍

​	当我们在 Kubernetes 中谈论 API 时，我们经常会使用 4 个术语：**groups** 、**versions**、**kinds** 和 **resources**。

### groups和versions

​	Kubernetes 中的 **API groups**简单来说就是相关功能的集合。每groups都有一个或多个**version**，顾名思义，它允许我们随着时间的推移改变 **API** 的职责。

### kind和resources

​	每个 API group-version包含一个或多个 API 类型，我们称之为 **Kinds**。虽然一个 Kind 可以在不同版本之间改变表单内容，但每个表单必须能够以某种方式存储其他表单的所有数据（我们可以将数据存储在字段中，或者在注释中）。 这意味着，使用旧的 API 版本不会导致新的数据丢失或损坏。更多 API 信息，请参阅 [Kubernetes API 指南](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md)。

​	 resources（资源） 只是 API 中的一个 Kind 的使用方式。通常情况下，Kind 和 resources 之间有一个一对一的映射。 例如，`pods` 资源对应于 `Pod` 种类。但是有时，同一类型可能由多个资源返回。例如，`Scale` Kind 是由所有 `scale` 子资源返回的，如 `deployments/scale` 或 `replicasets/scale`。这就是允许 Kubernetes HorizontalPodAutoscaler(HPA) 与不同资源交互的原因。然而，使用 CRD，每个 Kind 都将对应一个 resources。

注意：resources 总是用小写，按照惯例是 Kind 的小写形式。

**GVK = Group Version Kind  GVR = Group Version Resources**

### [那么，这些术语如何对应到 Golang 中的实现呢？](https://cloudnative.to/kubebuilder/cronjob-tutorial/gvks.html#那么这些术语如何对应到-golang-中的实现呢)

当我们在一个特定的群组版本 (Group-Version) 中提到一个 Kind 时，我们会把它称为 **GroupVersionKind**，简称 GVK。与 资源 (resources) 和 GVR 一样，我们很快就会看到，每个 GVK 对应 Golang 代码中的到对应生成代码中的 Go type。

现在我们理解了这些术语，我们就可以**真正**地创建我们的 API！

### [Scheme 是什么？](https://cloudnative.to/kubebuilder/cronjob-tutorial/gvks.html#scheme-是什么)

我们之前看到的 `Scheme` 是一种追踪 Go Type 的方法，它对应于给定的 GVK（ [godocs](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime#Scheme)）。

例如，假设我们将 `"tutorial.kubebuilder.io/api/v1".CronJob{}` 类型放置在 `batch.tutorial.kubebuilder.io/v1` API 组中（也就是说它有 `CronJob` Kind)。

然后，我们便可以在 API server 给定的 json 数据构造一个新的 `&CronJob{}`。

```json
{
    "kind": "CronJob",
    "apiVersion": "batch.tutorial.kubebuilder.io/v1",
    ...
}
```

或当我们在一次变更中去更新或提交 `&CronJob{}` 时，查找正确的组版本。

## 2.3创建一个api

搭建一个新的kind和控制器可以用kubebuilder create api

```bash
kubebuilder create api --group batch --version v1 --kind CronJob
```

当第一次为每个组-版本调用这个命令的时候，它将会为新的组-版本创建一个目录

它也为`CronJob` Kind 添加了一个文件，`api/v1/cronjob_types.go`。每次用不同的 kind 去调用这个命令，它将添加一个相应的新文件。

我们导入`meta/v1` API 组，通常本身并不会暴露该组，而是包含所有 Kubernetes 种类共有的元数据。

```go
package v1

import (
	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
)
```

​	为种类的 Spe c和 Status 定义类型。Kubernetes 功能通过使期待的状态(`Spec`)和实际集群状态(其他对象的 `Status`)保持一致和外部状态，然后记录观察到的状态(`Status`)。 因此，每个 *functional* 对象包括 spec 和 status 。很少的类型，像 `ConfigMap` 不需要遵从这个模式，因为它们不编码期待的状态， 但是大部分类型需要做这一步。

```
// 编辑这个文件！这是你拥有的脚手架！
// 注意: json 标签是必需的。为了能够序列化字段，任何你添加的新的字段一定有json标签。

// CronJobSpec 定义了 CronJob 期待的状态
type CronJobSpec struct {
    // 插入额外的 SPEC 字段 - 集群期待的状态
    // 重要：修改了这个文件之后运行"make"去重新生成代码
}

// CronJobStatus 定义了 CronJob 观察的的状态
type CronJobStatus struct {
    // 插入额外的 STATUS 字段 - 定义集群观察的状态
    // 重要：修改了这个文件之后运行"make"去重新生成代码
}
```

定义与实际种类相对应的类型，`CronJob` 和 `CronJobList` 。 `CronJob` 是一个根类型, 它描述了 `CronJob` 种类。像所有 Kubernetes 对象，它包含 `TypeMeta` (描述了API版本和种类)，也包含其中拥有像名称,名称空间和标签的东西的 `ObjectMeta` 。

`CronJobList` 只是多个 `CronJob` 的容器。它是批量操作中使用的种类，像 LIST 。

通常情况下，我们从不修改任何一个 -- 所有修改都要到 Spec 或者 Status 。

 `+kubebuilder:object:root` 注释被称为标记。知道它们充当额外的元数据， 告诉[controller-tools](https://github.com/kubernetes-sigs/controller-tools)(我们的代码和YAML生成器)额外的信息。 这个特定的标签告诉 `object `生成器这个类型表示一个种类。然后，`object` 生成器为我们生成 这个所有表示种类的类型一定要实现的[runtime.Object](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime?tab=doc#Object)接口的实现。

```go
// +kubebuilder:object:root=true
// +kubebuilder:subresource:status
// CronJob 是 cronjobs API 的 Schema
type CronJob struct {
    metav1.TypeMeta   `json:",inline"`
    metav1.ObjectMeta `json:"metadata,omitempty"`

    Spec   CronJobSpec   `json:"spec,omitempty"`
    Status CronJobStatus `json:"status,omitempty"`
}

// +kubebuilder:object:root=true
// CronJobList 包含了一个 CronJob 的列表
type CronJobList struct {
    metav1.TypeMeta `json:",inline"`
    metav1.ListMeta `json:"metadata,omitempty"`
    Items           []CronJob `json:"items"`
}
```

最后，我们将这个 Go 类型添加到 API 组中。这允许我们将这个 API 组中的类型可以添加到任何[Scheme](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime?tab=doc#Scheme)。

```go
func init() {
    SchemeBuilder.Register(&CronJob{}, &CronJobList{})
}
```

## 2.4设计一个api

**原则**

​	序列化的字段**必须**是 `驼峰式` ，所以我们使用的 JSON 标签需要遵循该格式。我们也可以使用`omitempty` 标签来标记一个字段在空的时候应该在序列化的时候省略。

​	字段可以使用大多数的基本类型。数字是个例外：出于 API 兼容性的目的，只允许三种数字类型。对于整数，需要使用 `int32` 和 `int64` 类型；对于小数，使用 `resource.Quantity` 类型。

```
Quantity 是十进制数的一种特殊符号，它有一个明确固定的表示方式，使它们在不同的机器上更具可移植性。 你可能在 Kubernetes 中指定资源请求和对 pods 的限制时已经注意到它们。
它们在概念上的工作原理类似于浮点数：它们有一个 significand、基数和指数。它们的序列化和人类可读格式使用整数和后缀来指定值，就像我们描述计算机存储的方式一样。
例如，值 2m 在十进制符号中表示 0.002。 2Ki 在十进制中表示 2048 ，而 2K 在十进制中表示 2000。 如果我们要指定分数，我们就换成一个后缀，让我们使用一个整数：2.5 就是 2500m。
有两个支持的基数：10 和 2（分别称为十进制和二进制）。十进制基数用 “普通的” SI 后缀表示（如 M 和 K ），而二进制基数用 “mebi” 符号表示（如 Mi 和 Ki ）。 对比 megabytes vs mebibytes。
```

还有一个我们使用的特殊类型：`metav1.Time`。 它有一个稳定的、可移植的序列化格式的功能，其他与 `time.Time` 相同

引用

```go
package v1
Imports
import (
    batchv1beta1 "k8s.io/api/batch/v1beta1"
    corev1 "k8s.io/api/core/v1"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
)

```

**spec**

看看 spec。正如我们之前讨论过的，spec 代表所期望的状态，所以控制器的任何 “输入” 都会在这里。

通常来说，CronJob 由以下几部分组成：

- 一个时间表（ CronJob 中的 cron ）
- 要运行的 Job 模板（ CronJob 中的 Job ）

当然 CronJob 还需要一些额外的东西，使得它更加易用

- 一个已经启动的 Job 的超时时间（如果该 Job 执行超时，那么我们会将在下次调度的时候重新执行该 Job）。
- 如果多个 Job 同时运行，该怎么办（我们要等待吗？还是停止旧的 Job ？）
- 暂停 CronJob 运行的方法，以防出现问题。
- 对旧 Job 历史的限制

请记住，由于我们从不读取自己的状态，我们需要有一些其他的方法来跟踪一个 Job 是否已经运行。我们可以使用至少一个旧的 Job 来做这件事。

我们将使用几个标记（`// +comment`）来指定额外的元数据。在生成 CRD 清单时，[controller-tools](https://github.com/kubernetes-sigs/controller-tools) 将使用这些数据。我们稍后将看到，controller-tools 也将使用 GoDoc 来生成字段的描述。

未完待续。。。。

## 2.5controller中有什么？



