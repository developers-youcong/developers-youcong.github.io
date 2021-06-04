---
title: centos7.8之防火墙常用命令
date: 2020-09-09 20:48:09
tags: "Linux"
---

查看防火墙状态:
```
systemctl status firewalld

```
<!--more-->
查看防火墙规则:
```
firewall-cmd --list-all

```

防火墙开启端口命令:
```
firewall-cmd --zone=public --add-port=80/tcp --permanent

```

防火墙关闭端口命令:
```
firewall-cmd --remove-port=80/udp --permanent

```

防火墙重启命令:
```
firewall-cmd --reload

```


查看防火墙已开启端口:
```
firewall-cmd --list-ports

```