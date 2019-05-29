---
title: JSON.stringify()和JSON.parse()的作用
date: 2019-03-22 16:44:36
tags: "javascript"
---

(1)JSON.stringify() 从一个对象中解析出字符串

JSON.stringify({"a":"1","b":"2"})

结果是："{"a":"1","b":"2"}"



(2)JSON.parse()从一个字符串中解析出JSON对象

var str = '{"a":"1","b":"2"}';

JSON.parse(str);

结果是：Object{a:"1",b:"2"}

应用场景:
针对(1)，比如后台Java对应的RequestMapping参数列表中的参数为一个对象时，前台多个传输需要经过JSON.stringify()处理，否则会出现参数解析异常。

针对(2)，比如向后台请求，后台返回一大堆字符串，这时前台页面渲染需要将其以对象的形式展现，这时可以用到JSON.parse()。