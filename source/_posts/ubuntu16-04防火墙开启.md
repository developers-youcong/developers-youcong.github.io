---
title: ubuntu16.04防火墙开启
date: 2020-09-26 12:23:55
tags: "Linux"
---

ubuntu16.04防火墙开启和常用命令:
```
sudo apt-get install ufw #安装防火墙
sudo ufw status #防火墙状态
sudo ufw enable #开启防火墙
sudo ufw allow 22  #开启端口
sudo ufw reload  #重启防火墙
sudo ufw delete allow 8001/tcp #关闭端口

```
<!--more-->
参考资料:
[ubuntu16.04防火墙开启](https://blog.csdn.net/qq_36938617/article/details/95234909)