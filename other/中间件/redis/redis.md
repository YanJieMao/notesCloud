## k3s简介

K3S是一个完全符合Kubernetes的发行版。可以使用单一二进制包安装（不到 100MB），安装简单，内存只有一半，最低0.5G内存就能运行。

## 安装

使用官方脚本安装K3S，同时会安装其他实用程序，包括`kubectl`、`crictl`、`ctr`、`k3s-killall.sh`和`k3s-uninstall.sh`；

```bash
curl -sfL http://rancher-mirror.cnrancher.com/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -
```

安装完成后会有下列提示信息

```bash
Complete!
[INFO]  Skipping /usr/local/bin/kubectl symlink to k3s, command exists in PATH at /usr/bin/kubectl
[INFO]  Skipping /usr/local/bin/crictl symlink to k3s, command exists in PATH at /usr/bin/crictl
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, command exists in PATH at /usr/bin/ctr
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
[INFO]  systemd: Starting k3s

```

查看一下运行状态

```bash
[root@dev-intern-1 ~]# systemctl status k3s
● k3s.service - Lightweight Kubernetes
   Loaded: loaded (/etc/systemd/system/k3s.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2021-03-26 08:59:18 CST; 4min 17s ago
     Docs: https://k3s.io
  Process: 302068 ExecStartPre=/sbin/modprobe overlay (code=exited, status=0/SUCCESS)
  Process: 302059 ExecStartPre=/sbin/modprobe br_netfilter (code=exited, status=0/SUCCESS)
 Main PID: 302071 (k3s-server)
    Tasks: 100
   Memory: 1.0G
   CGroup: /system.slice/k3s.service
           ├─302071 /usr/local/bin/k3s server
           ├─302196 containerd
           ├─302970 /var/lib/rancher/k3s/data/a6857be08414815b83ca6b960373efd98879a0b286fb24cb62b1c5fdbf3a8cb5/bin/containerd-shim-runc...
           ├─303077 /var/lib/rancher/k3s/data/a6857be08414815b83ca6b960373efd98879a0b286fb24cb62b1c5fdbf3a8cb5/bin/containerd-shim-runc...
           ├─303089 /var/lib/rancher/k3s/data/a6857be08414815b83ca6b960373efd98879a0b286fb24cb62b1c5fdbf3a8cb5/bin/containerd-shim-runc...
           ├─304279 /var/lib/rancher/k3s/data/a6857be08414815b83ca6b960373efd98879a0b286fb24cb62b1c5fdbf3a8cb5/bin/containerd-shim-runc...
           └─304347 /var/lib/rancher/k3s/data/a6857be08414815b83ca6b960373efd98879a0b286fb24cb62b1c5fdbf3a8cb5/bin/containerd-shim-runc...
```

