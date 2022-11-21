---
title: node.js如何打开指定浏览器
date: 2022-02-25 19:09:29
tags: "Node.js"
---


## 一、使用child_process
<!--more-->

```
var c = require('child_process');
// 使用默认浏览器打开
//c.exec('start https://www.youcongtech.com');
// 使用指定浏览器打开
c.exec('start chrome https://www.youcongtech.com');

```

## 二、使用open
```

var open = require('open');
// 使用默认浏览器打开
//open('https://www.youcongtech.com');
// 使用指定浏览器打开
open('https://www.youcongtech.com', 'firefox');

```

## 三、使用opn
```
const opn = require('opn');
// 使用默认浏览器打开
//opn('https://www.youcongtech.com');
// 使用指定浏览器打开
opn('https://www.youcongtech.com', {app: 'firefox'});

```

## 四、自定义封装函数(针对打开浏览器访问指定的URL)
```
var fs     = require('fs')
var os     = require('os')
var cp     = require('child_process')
var path    = require('path')
var open = function(url) {
 var userInfo  = os.userInfo()
 var chromePath = path.join(userInfo.homedir, 'Local Settings\\Application Data\\Google\\Chrome\\Application\\chrome.exe')
 var openByIE  = function() {
  cp.exec('start chrome ' + url, function(err, stdout, stderr) {
   if (err) {
    console.log(err)
   }
  })
 }
 fs.stat(chromePath, function(err) {
  if (err) {
   openByIE()
   return
  }
  cp.exec('start firefox ' + url, function(err, stdout, stderr) {
   if (err) {
    openByIE()
    return
   }
  })
 })
}

open("https://youcongtech.com")


```


## 五、注意问题
**常见错误:**
```
Error: Cannot find module 'xx'

```

xx是对应的模块，缺失对应的模块是无法调用对应的API的。

**解决办法(安装模块):**
```
npm install xx

```