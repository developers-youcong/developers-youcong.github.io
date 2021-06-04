---
title: Linux git免密登录
date: 2021-01-13 19:53:42
tags: "Linux"
---
因为某种原因，内网的git需要对外开放，为了保障git代码库的安全。
我让所有的仓库均为不可见，相当于登录才能看到对应的代码仓库，不登录看不到，之前是不登陆也能clone到本地，因为是内网，不怕外界侵入。但因此我的部署脚本需要重复输入用户名和密码，为了只输入一次，我执行了如下命令:
```
git config --global credential.helper store

git config --list
```
<!--more-->

参考链接:
[Linux 的Git客户端免密登录](https://www.jianshu.com/p/d8a6dffd08da)