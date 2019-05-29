---
title: 'E:dpkg was interrupted, you must manually run''dpkg配置''to correct the problem.'
date: 2019-04-10 15:13:12
tags: "Linux"
---
执行sudo apt-get install安装对应的软件出现如下错误

详细错误信息:
```
E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?

```

错误原因:是因为引用错误的链接导致的。

解决办法(删除这些引用即可):
```
cd /var/lib/dpkg/updates
rm -r ./*

```

删除完后，执行sudo apt-get update即可，这时就可以正常安装软件了。

参考解决办法链接:
[14.04消息'E:dpkg was interrupted, you must manually run'dpkg配置'to correct the problem.'](https://www.helplib.com/ubuntu/article_158303)




