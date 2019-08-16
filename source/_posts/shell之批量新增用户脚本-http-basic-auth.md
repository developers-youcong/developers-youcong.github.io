---
title: shell之批量新增用户脚本(http-basic-auth)
date: 2019-07-22 11:14:32
tags: "Linux"
---

user.txt(用户名记录文件)
```
test001@163.com
test002@163.com

```


user.sh(shell脚本):
```

for line in `cat user.txt`
do
   echo $line "u"$line
   printf "$line:$(openssl passwd -crypt $line)\n" >> conf.d/passwd

done


```

执行完毕后，就可以在passwd看到对应的记录。


应用场景:
比如我开发某个系统，希望有一个双重验证，第一次访问比如有一个HTTP Basic Auth认证(认证一次，浏览器有缓存，就无需再重新验证)，第二次如果你想使用系统的服务的话，还需要登录。
<!--more-->

