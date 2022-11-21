---
title: 'Error: Failed to download metadata for repo ''appstream'''
date: 2022-07-16 14:19:08
tags: "Linux"
---

## 一、详细错误信息
<!--more-->
```
CentOS Linux 8 - AppStream                                                                                                 
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist


```

二、解决办法

### 1.进入yum的repos目录
```
cd /etc/yum.repos.d/

```

### 2.修改所有的CentOS文件内容
```
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*

sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

```

### 3.更新yum源为阿里镜像
```
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo

yum clean all

yum makecache

```

### 4.yum安装测试是否可以yum安装
```
yum install git –y

```