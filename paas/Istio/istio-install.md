## 下载istio

从github下载https://github.com/istio/istio/releases/tag/1.8.2

然后把istioctl添加到环境变量中，或者直接复制到、/usr/bin中

## 安装demo

```bash
istioctl manifest apply --set profile=demo
```

> :warning:在安装之前最好配置好docker阿里云的镜像加速