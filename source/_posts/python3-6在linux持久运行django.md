---
title: python3.6在linux持久运行django
date: 2019-03-19 14:17:45
tags: "Python"
---

最近线上运行一个OnlineJudgeServer的项目，通过python manage.py runserver 0.0.0.0:8090运行，如果关闭当前窗口，实际就相当于关闭了这个进程。
<!--more-->
之前说过通过nuhub可以实现在Linux持久运行的目的。

如果你的nohub出现 nohub命令找不到，那么你可以执行如下这个命令:
```
/usr/bin/nohup python manage.py runserver 0.0.0.0:8090 > system.log 2>&1 &

```

但是由于python版本不一样，对应的django也会存在差异，报了些错误,主要是关于djaon版本问题(与Python版本也有关)。

最后通过如下命令解决该问题:
```
/usr/bin/nohup python3.6 manage.py runserver 0.0.0.0:8090 > system.log 2>&1 &

```

