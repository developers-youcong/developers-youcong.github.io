---
title: node.js服务端程序在Linux上持久运行
date: 2019-03-05 14:04:07
tags: "Node.js"
---
如果要想在服务端部署node.js程序，让其持久化运行，就不能单单使用npm start命令运行，当然了，这样运行是毫无问题的，但是当关闭xshell窗口或者是关闭进程的时候(其实关闭xshell窗口相当于默认关闭进程)，就无法访问对应的node.js服务端程序了。

那么该如何才能持久访问呢？

其实也就两步
<!--more-->
#### 第一步安装forever
```
npm install forever 或者 npminstall -g forever
```

#### 第二步运行对应的js
```
forver start index.js

```

##### 注意(你可能会遇到如下错误):

错误信息:
forever: command not found

原因:以Windows来说，通常这种错误是因为没有配置好环境变量，解决方案也很简单就是配置好环境变量或者是使用绝对路径

解决方式(Linux演示，这里我使用绝对路径):

##### 如何找到绝对路径呢？

通过该命令可以获取node.js的安装模块,npm list -g --depth 0
├── ali-oss@6.1.0
├── forever@0.15.3
└── npm@6.4.1

再通过关键字搜索 find / -name forever
/home/youcong/mock-github-api/node_modules/forever
/home/youcong/mock-github-api/node_modules/forever/lib/forever
/home/youcong/mock-github-api/node_modules/forever/bin/forever
/home/youcong/mock-github-api/node_modules/.bin/forever
/home/youcong/nodejs/lib/node_modules/forever
/home/youcong/nodejs/lib/node_modules/forever/lib/forever
/home/youcong/nodejs/lib/node_modules/forever/bin/forever
/home/youcong/nodejs/bin/forever

最后通过/home/youcong/mock-github-api/node_modules/forever/bin/forever start index.js 即可实现node.js服务端程序在Linux上持久运行。

##### forever常用命令

forever start app.js //启动程序

forever stop app.js //关闭程序

forever start -l forever.log -o out.log -e err.log app.js //启动程序并输出日志

forever restart app.js //重启程序

forever list //查看正在运行的进程


###### 参考链接:
forever:command not found:https://blog.csdn.net/xgbm_k/article/details/78132293
node.js在Linux上如何持久运行:https://blog.csdn.net/shakdy/article/details/82938679
node.js后台运行方法:https://blog.csdn.net/zdyueguanyun/article/details/79043483


