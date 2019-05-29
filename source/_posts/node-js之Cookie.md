---
title: node.js之Cookie
date: 2019-03-13 22:27:45
tags: "Node.js"
---
最近还是用node.js比较多，今天正好遇见一个问题，还是关于Cookie。
<!--more-->
node.js中如何实现cookie(以express框架为例):
```
"use strict";
 
var express = require("express");
var cookieParser = require("cookie-parser");
var util = require("util");
 
var app = express();
app.use(cookieParser());
 
app.get("/test",function (req, res) {
    console.log("Cookies: " + util.inspect(req.cookies));
	
	res.send("cookie");
	
});
 
app.listen(8081);


```

控制台输出结果为:
```
Cookies: { Hm_lvt_2edaee7dab677cdec491683758d1e378: '1552046206,1552134942,1552485731',
  Hm_lpvt_2edaee7dab677cdec491683758d1e378: '1552485731',
  add: [ null, null, null, null, null, null, null ] }

```


如果不使用express的话，那么原生node.js是如何实现的呢？代码如下:
```
var http = require('http');

http.createServer(function (req, res) {

    // 获得客户端的Cookie

    var Cookies = {};

    req.headers.cookie && req.headers.cookie.split(';').forEach(function( Cookie ) {

        var parts = Cookie.split('=');

        Cookies[ parts[ 0 ].trim() ] = ( parts[ 1 ] || '' ).trim();

    });

    console.log(Cookies)

    // 向客户端设置一个Cookie

    res.writeHead(200, {

        'Set-Cookie': 'myCookie=test',

        'Content-Type': 'text/plain'

    });

    res.end('Hello World\n');

}).listen(8000);

 

console.log('Server running at http://127.0.0.1:8000/');

```
'
cookie并不是万能的，相反它有一定的安全隐患，为此node.js有一种cookie签名实现，源码如下:
```
const express = require('express');
const cookieParser = require('cookie-parser');

//随机生成的字符串
var signStr = 'xadsafeowirw'

var app = express();

//需要将密匙传给cookieParser, 在接收数据的时候，进行解析。
app.use(cookieParser(signStr));

app.use('/', function (req, res) {
    //将密匙字符串赋值给req.secret,可以省略，在上面cookieparser()时会自动对secret赋值
    req.secret=signStr;

    //返回给浏览器的cookie, 这就是传说中的种cookie了
    //如果需要开启签名，第三个参数对象signed 设置为true.
    //由于cookie的大小限制4k，而签名后的cookie体积会增加，所以重要的cookie才签名
    res.cookie('cookiename', 'youcong', {signed: true, maxAge: 3600})

    //有没有签名的cookie，获取方式不一样。
    console.log('无签名', req.cookies);
    console.log('带签名',req.signedCookies);
    res.send('ok')
})
app.listen(8090);

```

顺便再补充一下node.js的session实现,代码如下:
```
const express = require('express');
const cookieParser = require('cookie-parser');
const cookieSession = require('cookie-session');

var app = express();


app.use(cookieParser());

//cookieSession 必须放在cookieParser后面
app.use(cookieSession({
    //session的秘钥，防止session劫持。 这个秘钥会被循环使用，秘钥越长，数量越多，破解难度越高。
    keys: ['aaa', 'bbb', 'ccc'],
    //session过期时间，不易太长。php默认20分钟
    maxAge: 60*60,
    //可以改变浏览器cookie的名字
    name: 'session'
}));

app.use('/', function (req, res) {

    //假设使用count记录用户访问的次数
   if(req.session['count'] == null) {
       req.session['count'] = 1;
   }else{
       req.session['count']++;
   }
   console.log(req.session['count'])
    res.send('ok')
})
app.listen(8090)



```




参考资料如下:
node.js操作Cookie，让你清楚了解cookie存入过程:https://blog.csdn.net/sinat_18474835/article/details/79987282
Node.js学习（15）-Cookie:https://blog.csdn.net/sunhuansheng/article/details/82356129
node学习之cookie和session:https://www.cnblogs.com/lijinwen/p/7898159.html