## 1.建立镜像

### 1.拉取官方镜像

```bash
docker pull mysql:5.7 //拉取5.7版本的
docker pull mysql   //拉取最新版本的
```

### 2.检查是否拉取完成

```bash
sudo docker images
```



### 3.一般数据库不需要建立目录映射

```bash
sudo docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=1997 -d mysql:5.7
```

- –name：容器名，此处命名为`mysql`
- -e：配置信息，此处配置mysql的root用户的登陆密码
- -p：端口映射，此处映射 主机3306端口 到 容器的3306端口



------

ps:如果需要建立映射

```
sudo docker run -p 3306:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=1997 \
-d mysql:5.7
```

- -v：主机和容器的目录映射关系，":"前为主机目录，之后为容器目录

#### 4.检查容器运行是否正常

```bash
docker container ls
```

- 可以看到容器ID，容器的源镜像，启动命令，创建时间，状态，端口映射信息，容器名字

## 2.连接mysql

### 1.docker容器内连接mysql

```bash
sudo docker exec -it mysql bash
mysql -uroot -p1997
```

### 2.用图形化的管理工具连接mysql