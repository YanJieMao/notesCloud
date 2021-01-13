# istio边车注入

## istio边车注入过程

​	注入Sidecar的时候会在生成pod的时候附加上两个容器：istio-init，istio-proxy。

​	istio-init这个容器属于k8s中的Init容器，主要用于设置iptables规则，让出入流量都转由Sidecar进行处理。istio-proxy是基于Envoy实现的一个网络代理容器。

​	在使用Sidecar自动注入的时候只需要给对应的应用部署的命名空间打个istio-injection = enabled标签，这个命名空间中新建的任何Pod都会被Istio注入Sidecar。

## istio边车注入原理

​	Sidecar注入主要是依托k8s的准入控制器Admission Controller来实现的。

​	准入控制器会拦截Kubernetes API Server收到的请求，拦截发生在认证和鉴权完成之后，对象进行持久化之前。主要两种类型的webhook：**ValidatingAdmissionWebhook**和**MutatingAdmissionWebhook**。

​	**ValidatingAdmissionWebhook**可以根据自定义的准入策略决定是否拒绝请求，

​	**MutatingAdmissionWebhook**可以根据自定义配置来对请求进行编辑。

​	**MutatingAdmissionWebhook** 在资源被持久化到 etcd 前，根据规则将其请求拦截，拦截规则定义在 **MutatingWebhookConfiguration** 中。**MutatingAdmissionWebhook** 通过对 webhook server 发送准入请求来实现对资源的更改。

​	在一个命名空间中设置了 istio-injection=enabled 标签，webhook 被启用后，任何新的 pod 都有将在创建时自动添加 sidecar。

​	总之：istio 通过webhook来拦截pods的创建请求，然后通过webhook注入边车pod,从而实现微服务治理


