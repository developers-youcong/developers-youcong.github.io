---
title: '/usr/bin/xauth:  file /home/user/.Xauthority does not exist'
date: 2019-06-19 20:22:51
tags: "Linux"
---

错误信息如下:
```
/usr/bin/xauth: file /home/user/.Xauthority does not exist

```
<!--more-->
**错误原因:**
是因为添加用户时没有授权对应的目录，仅仅执行了useradd user而没有授权对应的家目录


直接解决办法如下(执行如下命令，以后就登录到终端上就不会出现上面的错误信息):
```
chown username:username -R /home/user_dir

```

不过一般是可以避免这种情况的出现，添加用户执行如下命令即可:
```
useradd username -m (-m 相当于会创建对应的用户家目录)

usermod -s /bin/bash username (指定shell，否则会非常不便于终端操作)

```


