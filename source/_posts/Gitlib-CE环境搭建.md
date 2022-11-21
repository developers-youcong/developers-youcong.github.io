---
title: Gitlib-CE环境搭建
date: 2022-02-12 13:46:36
tags: ["Linux","项目管理","Git"]
---

## 一、配置镜像
<!--more-->
### 1.备份
```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup


```

### 2.下载
```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
# 或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo


```

### 3.生成缓存
```
yum makecache

```

## 二、安装Gitlib
```
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

yum -y install gitlab-ce

```
## 三、配置Gitlib
```
cd /etc/gitlab/
vi gitlab.rb

external_url'http://gitlab.example.com' #域名或端口(如果是端口，需写为http://192.168.0.1:9090

```

## 四、初始化
```
gitlab-ctl reconfigure

```

## 五、启动Gitlab
```
gitlab-ctl start

```
参考资料列表:
[Linux初装gitlab初始默认密码](https://blog.csdn.net/timonium/article/details/119451755)
[如何在CentOS 7上安装和配置GitLab CE](https://curder.gitbooks.io/blog/content/centos/how-to-install-and-configure-gitlab-ce-on-centos-7.html
)
[gitlab的安装与修改端口配置](https://www.cnblogs.com/heartxkl/p/12943904.html)
