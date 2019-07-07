---
title: 'git之fatal: Could not read from remote repository'
date: 2019-06-30 21:32:22
tags: "Git"
---

问题背景:
在git bash中使用hexo g -d命令进行文章发布

详细错误信息:
```
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Connection reset by 13.250.177.223 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

    at ChildProcess.<anonymous> (E:\Hexo\node_modules\hexo-util\lib\spawn.js:37:17)
    at emitTwo (events.js:126:13)
    at ChildProcess.emit (events.js:214:7)
    at ChildProcess.cp.emit (E:\Hexo\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:915:16)
    at Socket.stream.socket.on (internal/child_process.js:336:11)
    at emitOne (events.js:116:13)
    at Socket.emit (events.js:211:7)
    at Pipe._handle.close [as _onclose] (net.js:561:12)

```
错误原因是因为ssh key有问题，连接不上服务器。

<!--more-->
于是我参考如下链接，一步一步操作，最终解决了这个问题:
[git遇到的问题之“Please make sure you have the correct access rights and the repository exists.”](https://blog.csdn.net/jingtingfengguo/article/details/51892864)


虽然已经有了问题的解决答案，但是我觉得还是需要列举一下相关步骤，梳理一下:

1.重新在git设置一下身份的名字和邮箱
```
git config --global user.name "yourname"

git config --global user.email "your@email.com"

```
这里的yourname必须与github的用户名一致

这里your@email.com必须与github登录邮箱一致


2.删除.ssh文件夹下的known_hosts文件(该文件主要作用是域名解析)

3.ssh-keygen -t rsa -C "your@email.com(填写github对应的邮箱)"

一路回车即可，无需输入

4.进入github设置界面

![](git之fatal-Could-not-read-from-remote-repository/01.png)

新增SSH key将id_rsa.pub内容添加到如图中的Key上(title可任意命名):
![](git之fatal-Could-not-read-from-remote-repository/02.png)

5.最后完美解决这个问题