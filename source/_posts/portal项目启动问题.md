---
title: portal项目启动问题
date: 2019-12-04 22:33:00
tags: "Java"
---
错误信息:
```
Disconnected from the target VM, address: '127.0.0.1:58909', transport: 'socket'

Process finished with exit code -1

```

解决办法:
替换application-qa.properties文件，并将application.properties上面的profile指定为qa,并启动PortalApplication.java即可解决该问题。
<!--more-->