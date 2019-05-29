---
title: npm run dev 在Linux上持久运行
date: 2019-03-16 15:32:42
tags: "Linux"
---

关于node.js应用程序如何持久运行，我在[node.js服务端程序在Linux上持久运行](https://developers-youcong.github.io/2019/03/05/node-js%E6%9C%8D%E5%8A%A1%E7%AB%AF%E7%A8%8B%E5%BA%8F%E5%9C%A8Linux%E4%B8%8A%E6%8C%81%E4%B9%85%E8%BF%90%E8%A1%8C/)用过。

这次主要是针对是一个vue.js应用程序。
<!--more-->
vue.js应用程序通常运行命令是npm run dev。如果是在命令行输入该命令，则会出现如下信息:
```
 DONE  Compiled successfully in 1140ms                                                                          15:13:02

 I  Your application is running here: http://0.0.0.0:8081

```

假定如果关闭当前窗口则发现进程随之关闭，那么如何保证其持久运行，不会因为关闭窗口造成进程关闭，还是需要用到nohub这个Linux命令。

关于这个命令我在[springboot打成的jar包如何在Linux上持久运行](https://developers-youcong.github.io/2019/02/23/springboot%E6%89%93%E6%88%90%E7%9A%84jar%E5%8C%85%E5%A6%82%E4%BD%95%E5%9C%A8Linux%E4%B8%8A%E6%8C%81%E4%B9%85%E8%BF%90%E8%A1%8C/)用过

该vue.js应用程序同样适用。

如果想记录日志，请按照如下执行(一定要在package.json同级目录或者是当前项目根目录):

```
touch my.log
chmod u+w my.log
nohup npm run dev > my.log 2>my.log &
```

如果不想记录日志，如下:
```
nohup npm run dev >/dev/null 2>&1 &
exit

```

参考资料:
[让npm run dev在Linux后台 持久运行](https://blog.csdn.net/chanlingmai5374/article/details/80762983?utm_source=blogxgwz7)