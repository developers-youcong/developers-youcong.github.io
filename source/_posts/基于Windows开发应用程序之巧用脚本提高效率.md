---
title: 基于Windows开发应用程序之巧用脚本提高效率
date: 2022-03-15 21:21:36
tags: "计算机"
---
一般开发应用微服务，要么基于Windows平台，要么基于Mac，或者基于Linux桌面端(如Ubuntu等)。以Windows为例，有些时候与前端小伙伴联调接口时，由于我这边需要开发，防止重启导致前端小伙伴调接口过程中出问题(如连接不上等)，我可以通过在我本地持久化部署来达到本地开发与部署不冲突的目的。那么如何基于Windows如何本地部署呢？我这里编写了对应的bat脚本，可供小伙伴参考:
<!--more-->

run.bat(文件名)
```
@echo off
start javaw -Xms512m -Xmx1024m -XX:PermSize=256m -XX:MaxPermSize=512m -XX:MaxNewSize=512m  -jar Blog-Web.jar >> StartupLog.log  2>&1 &
exit

```
通过上面脚本可以实现双击自启动。那么启动之后我想替换怎么办？一般的常规做法是执行如下命令:
```
netstat -ano | findstr 端口 #查询端口进程
tasklist | findstr 进程号 #找到端口进程占用的应用
taskkill -PID 进程号 -F #杀死端口进程

```

但我觉得这样太麻烦了，不够自动化，还得手动复制执行，于是我编写对应的脚本实现输入对应的端口就能自动杀死对应的进程，脚本内容如下:
kill.bat(文件名)
```
@echo off
setlocal enabledelayedexpansion
set /p port=please input port：
for /f "tokens=1-5" %%a in ('netstat -ano ^| find ":%port%"') do (
    if "%%e%" == "" (
        set pid=%%d
    ) else (
        set pid=%%e
    )
    echo !pid!
    taskkill /f /pid !pid!
)
pause

```
我在[我对加班的看法](https://youcongtech.com/2022/03/13/%E6%88%91%E5%AF%B9%E5%8A%A0%E7%8F%AD%E7%9A%84%E7%9C%8B%E6%B3%95/)这篇文章提到过如何提高工作效率，我总结了8点，其中就有一点内容为"能自动化就不要手动或半自动化"。希望此文能够对大家有所帮助。