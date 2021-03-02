# Git操作

## 1.Git基本操作

### 1.1基础命令

1.git init

初始化Git仓库

2.git add 

添加更改到暂存区

3.git commit

提交更改到本地仓库

4.git push

推送代码到远程仓库

5.git clone xxxxx

clone 远程仓库到本地

6.git pull 

从远程仓库拉取

### 1.2git基础配置

配置用户名邮箱地址

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
#ps:设置单个仓库把--global去掉就可以了
```

### 1.3git多分枝操作

查看分支

```bash
git branch
```

切换分支

```bash
#假如有main dev 分支，现在在main分支
git checkout dev
```

创建新分支 

```bash
#创建一个test新分支
git checkout -b test
```

## 2.git 协同