---
title: Hexo博客推送至Github报错问题之解决
date: 2023-05-08 00:29:50
tags: ["计算机","GitHub","Git"]
---

**问题背景:**
执行Hexo推送至Github报错。执行命令如下:
<!--more-->
```
hexo d

```

**核心错误信息:**

```
 37 files changed, 73 insertions(+), 73 deletions(-)
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:uNiVztksCsDhcc0u9e8BujQXVUpKZIDTMczCvj3tD2s.
Please contact your system administrator.
Add correct host key in /c/Users/youcong/.ssh/known_hosts to get rid of this message.
Offending RSA key in /c/Users/youcong/.ssh/known_hosts:1
RSA host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Error: @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:uNiVztksCsDhcc0u9e8BujQXVUpKZIDTMczCvj3tD2s.
Please contact your system administrator.
Add correct host key in /c/Users/youcong/.ssh/known_hosts to get rid of this message.
Offending RSA key in /c/Users/youcong/.ssh/known_hosts:1
RSA host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

    at ChildProcess.<anonymous> (D:\Hexo\node_modules\hexo-util\lib\spawn.js:37:17)
    at ChildProcess.emit (events.js:210:5)
    at ChildProcess.cp.emit (D:\Hexo\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:1021:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:283:5)


```


其中关键错误信息是:
```
Please contact your system administrator.
Add correct host key in /c/Users/youcong/.ssh/known_hosts to get rid of this message.
```

我找到了这个关键错误信息，采取了一种简单粗暴的办法，清理了know_hosts文件中的内容(其实，只需将Github相关内容清理即可)。

这时再重新执行Hexo博客推送Github命令即可，问题就解决了。

**推送成功显示如下:**
```
The file will have its original line endings in your working directory
On branch master
nothing to commit, working tree clean
remote: Resolving deltas: 100% (1253/1253), completed with 553 local objects.
Branch 'master' set up to track remote branch 'master' from 'git@github.com:developers-youcong/developers-youcong.github.io.git'.
To github.com:developers-youcong/developers-youcong.github.io.git
   d4d09038d8..4cf3151472  HEAD -> master
INFO  Deploy done: git


```