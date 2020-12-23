## 下载

下载连接

https://storage.googleapis.com/kubernetes-release/release/v1.20.0/bin/windows/amd64/kubectl.exe

## 配置

将下载的二进制文件存在某个文件夹

配置环境变量，将kubectl.exe所在的目录放置到path中

## 测试配置

运行

```powershell
kubectl version --client
```

测试配置是否正确

```bash
PS C:\Users\x> kubectl version --client
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.3", GitCommit:"1e11e4a2108024935e******", GitTreeState:"clean", BuildDate:"2020-10-14T12:50:19Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"windows/amd64"}
```

出现以上内容说明配置成功

