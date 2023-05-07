---
title: Cannot read property 'pickAlgorithm' of null
date: 2023-01-29 16:07:29
tags: "npm"
---


详细错误信息如下:
<!--more-->

```
npm ERR! Cannot read property 'pickAlgorithm' of null

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\youcong\AppData\Local\npm-cache\_logs\2023-01-14T07_40_35_979Z-debug.log

```

解决方案参考:
```
npm cache clear --force
npm install
```

相当于先清理缓存，再重新安装包。

参考链接:
https://blog.csdn.net/xiaoxiannvh/article/details/124487447