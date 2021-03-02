## 安装gcc

yum install gcc gcc-c++

## 下载

 wget https://npm.taobao.org/mirrors/node/v14.6.0/node-v14.6.0-linux-x64.tar.gz

## 解压 

 tar -xvf  node-v14.6.0-linux-x64.tar.gz

mv node-v14.6.0-linux-x64.tar.gz  /usr/local/node

## 修改环境变量

vi /etc/profile

添加下面

```
#set for nodejs  
export NODE_HOME=/usr/local/node   # node所在路径
export PATH=$NODE_HOME/bin:$PATH
```

source /etc/profile

## 测试

node -v

npm -v

## 安装vue脚手架

安装 vue-cli 脚手架   npm install -g vue-cli  // 加-g是安装到全局

npm install vue-cli

