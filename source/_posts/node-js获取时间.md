---
title: node.js获取时间
date: 2019-03-27 11:28:44
tags: "Node.js"
---

## 1.Node.js安装时间库
```
npm install -g silly-datetime 或 npm install silly-datetime
```

## 2.使用代码
```
var sd = require('silly-datetime');
var time=sd.format(new Date(), 'YYYY-MM-DD HH:mm');
console.log(time);

```

展示效果如下:
![](node-js获取时间/date.png)