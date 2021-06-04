---
title: >-
  o.s.c.a.n.c.NacosPropertySourceBuilder : get data from Nacos
  error,dataId:application-dev.yaml
date: 2020-09-02 21:21:39
tags: "Linux"
---

昨天部署项目到公司内部开发服务器上，部署显示是成功，结果出现了这样的错误:
```
o.s.c.a.n.c.NacosPropertySourceBuilder : get data from Nacos error,dataId:application-dev.yaml

```
<!--more-->
通常这样的错误是因为IDE对应的文件字符编码，改下字符编码就可以了。


但我本地是正常的，只是部署Linux出现问题。

最后每次对应的微服务启动，都会显示无法解析共享配置文件(application-dev.yml)

共享配置文件主要的作用是请求URL不拦截、负载均衡、feign等。

现在的问题是一直访问不到请求，表示被拦截，这样加上日志输出的错误原因，我因此判定就是解析不到的原因。

但为什么解析不到呢？
可能与Linux字符集有关。

查看Linux对应的全局字符文件(vim /etc/locale.conf):
```
LANG="zh_CN"

```

最后我加上如下:
```
LANG="zh_CN.UTF-8"

```

最后问题全部得到解决了。包括之前项目启动输出日志是乱码的问题都得到了解决。

由于之前操作Ubuntu16.04一直没有出现过这样的问题，这次操作的Linux系统是CentOS7.8。

看来有些时候字符集真的要命。