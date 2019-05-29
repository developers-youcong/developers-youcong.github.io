---
title: 普通用户使用Docker
date: 2019-03-27 16:54:05
tags: "Docker"
---

## 1.查询是否有docker组：
```
cat /etc/group

```

如果没有可以通过该命令添加(一般默认是有的)

```
sudo groupadd docker
<!--more-->
```

## 2.将当前用户添加到docker组
```
sudo usermod -G docker $(USER)

```

例如:sudo usermod -G docker test


## 3.重启docker服务
```
sudo systemctl restart docker.service

```


参考资料如下:
普通用户使用Docker:https://blog.csdn.net/qq_36713450/article/details/83109477