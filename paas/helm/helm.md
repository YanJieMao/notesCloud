# helm

# 1.helm初识

 Helm 是 Kubernetes 的包管理器，您也可以在 [CNCF Helm 项目过程报告](https://www.cncf.io/cncf-helm-project-journey/)阅读详细的背景信息。

### 1.1使用helm

**三个概念**

- *Chart* 是一个Helm包，涵盖了需要在Kubernetes集群中运行应用，工具或者服务的资源定义。 把它想象成Kubernetes对应的Homebrew公式，Apt dpkg，或者是Yum RPM文件。
- *仓库* 是归集和分享chart的地方。类似于Perl的 [CPAN 归档](https://www.cpan.org/)或者 [Fedora 包数据库](https://fedorahosted.org/pkgdb2/)，只针对于Kubernetes包。
- *发布* 是在Kubernetes集群中运行的chart实例。一个chart经常在同一个集群中被重复安装。每次安装都会生成新的 *发布*。比如MySQL，如果想让两个数据库运行在集群中，可以将chart安装两次。每一个都会有自己的 *发布版本*，并有自己的 *发布名称*

总之：Helm在Kubernetes中安装的每一个 *charts*，都会创建一个新的 *发布*，想查找新chart，可以在Helm chart *仓库* 搜索。