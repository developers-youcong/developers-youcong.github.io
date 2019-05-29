---
title: 如何让Node.js运行在浏览器端
date: 2019-03-28 17:02:24
tags: "Node.js"
---

Node.js又称服务端JavaScript。
今天我为了解决一个问题，通过搜索引擎找到了如何将Node.js转成浏览器端可以运行的javascript。
尽管这种方式有其局限性，但是还是可以用的。
<!--more-->
## 1.安装库
```
npm install -g browserify

```


## 2.转换

```
 browserify test.js > index.js
 或
  browserify test > index.js
 或
 browserify test.js -o index.js 

```

以上三种方式均可行

参考资料:
如何让nodejs写的代码在浏览器里面运行:https://jingyan.baidu.com/article/48b37f8dda4cb11a646488b9.html
