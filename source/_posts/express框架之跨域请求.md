---
title: express框架之跨域请求
date: 2019-03-13 22:51:59
tags: "Node.js"
---
express.js跨域请求代码如下:
```
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

```
按照上面的代码，即可解决跨域问题。
