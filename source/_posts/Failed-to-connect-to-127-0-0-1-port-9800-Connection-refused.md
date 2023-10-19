---
title: 'Failed to connect to 127.0.0.1 port 9800: Connection refused'
date: 2023-08-04 20:09:13
tags: "Git"
---

## 一、错误信息
<!--more-->
```
Failed to connect to 127.0.0.1 port 9800: Connection refused

```

## 二、错误信息背景
今天研究了某段业务解决方案，研究完毕后，准备将其上传到我的问题解决方案库上，结果发现上传出现问题，最初因为开了代理导致提交不上(科学上网与Github提交有某种冲突，Clash Windows 开启代理)，结果我关闭了还提交不上(以往我提交都是关闭代理后再提交，都没有出现过这样的问题)。


## 三、解决问题

通常搜索关键词，大致可得到如下解决方案，一般情况按照如下执行，即可解决:
```
//取消http代理
git config --global --unset http.proxy
//取消https代理 
git config --global --unset https.proxy

```

但是我执行以后，没有反应，最后我考虑到可能是配置文件出了问题，然后我执行该命令:
```
vim ~/.gitconfig

```

果然发现是配置文件中多了配置代理(将其删除即可)，然后重新执行代码提交命令，成功提交代码至我的问题解决方案库上(问题得到解决)。