## 安装harbor私有镜像仓库

### 1.设置主机名

```bash
# 设置 Harbor 服务器主机名
hostnamectl set-hostname harbor.hao.com
```



### 2.安装docker服务

安装docker服务并配置阿里云加速器

1.安装基础软件包

```bash
yum -y install yum-utils device-mapper-persistent-data lvm2 
```

2.配置yum镜像仓库

```bash
yum-config-manager --add-repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

3.安装docker服务

```
yum list docker-ce --showduplicates|sort -r   # 查询docker版本
yum -y install docker-ce-xx.xx.xx   # 安装指定版本,根据生产环境自行选择
```

4.配置阿里云镜像加速器

\# 配置方式：
\# 登入https://www.aliyun.com 在阿里云控制台的容器镜像服务-->镜像加速器菜单中找到你的镜像加速地址。
\# 在 /etc/docker/daemon.json 中写入如下内容（文件不存在则创建改文件），镜像地址改为你的实际地址。

```
sudo mkdir -p /etc/docker
sudo cat /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://1qond552.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 3.安装docker-compose服务

官方地址：https://github.com/docker/compose/releases

```shell
# 1、登入 GitHub ，找到对应版本下载
curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

# 2、将下载后的文件放到 /usr/local/bin 目录下，并添加执行权限
chmod +x /usr/local/bin/docker-compose

# 3、查看版本
docker-compose -version
```



### 4.安装harbor服务

1.下载harbor软件包

方式一：登入官方地址下载对应版本： https://github.com/goharbor/harbor/releases 

方式二：直接下载：wget https://github.com/goharbor/harbor/releases/download/v2.1.2/harbor-offline-installer-v2.1.2.tgz

2.解压

将 harbor 服务加压到 /home 目录下，以下所有操作均以解压后目录为当前目录

```
tar xvf harbor-offline-installer-v2.1.2.tgz  -C /home/ && cd /home/harbor/
```

3.修改配置文件

3.1修改配置文件

```bash

# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: harbor.hao.com  ##########修改域名

########### 关闭http访问方式
#http:         ##########行注释掉
  # port for http, default is 80. If https enabled, this port will redirect to https port
  #port: 80   ##########行注释掉

########### 打开https访问方式
# https related config
https:#########取消注释
#   # https port for harbor, default is 443
   port: 443  #########取消注释
#   # The path of cert and key files for nginx
   certificate: /home/harbor/certs/harbor.crt        #########取消注释，填写实际路径
   private_key: /home/harbor/certs/harbor.key        #########取消注释，填写实际路径

.....
.....
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: Harbor12345  ######### admin用户登入密码

# Harbor DB configuration
database:
  # The password for the root user of Harbor DB. Change this before any production use.
  password: root123       ######### 数据库密码

# The default data volume
data_volume: /home/harbor/data   #########目录自己创建，根据实际情况填写
```

3.2创建相应文件夹

```
mkdir -p /home/harbor/certs /home/harbor/data
```

4.使用Openssh生成证书

```bash
# 1、生成证书，并保存到 /home/harbor/certs 目录下
openssl req -newkey rsa:4096 -nodes -sha256 -keyout /home/harbor/certs/harbor.key -x509 -out /home/harbor/certs/harbor.crt -subj /C=CN/ST=BJ/L=BJ/O=DEVOPS/CN=harbor.hao.com -days 3650

req 　　  产生证书签发申请命令
-newkey  生成新私钥
rsa:4096  生成秘钥位数
-nodes   表示私钥不加密
-sha256  使用SHA-2哈希算法
-keyout  将新创建的私钥写入的文件名
-x509 　　签发X.509格式证书命令。X.509是最通用的一种签名证书格式。
-out 指定要写入的输出文件名
-subj    指定用户信息
-days    有效期（3650表示十年）

# 2、查看证书
[root@harbor harbor]# ls certs/
harbor.crt  harbor.key
```

5.启动harbor服务

```bash
./install.sh
```

ps：如果一直在starting更换harbor版本