# kubebuilder

# 1.简介

Kubebuilder是一个使用CRD构建Kubernetes API的框架。

[Kubebuilder](https://link.zhihu.com/?target=https%3A//yq.aliyun.com/go/articleRenderRedirect%3Furl%3Dhttps%3A%2F%2Fgithub.com%2Fkubernetes-sigs%2Fkubebuilder) 是一个使用 CRDs 构建 K8s API 的 SDK，主要是：

- 提供脚手架工具初始化 CRDs 工程，自动生成 boilerplate 代码和配置；
- 提供代码库封装底层的 K8s go-client；

方便用户从零开始开发 CRDs，Controllers 和 Admission Webhooks 来扩展 K8s。