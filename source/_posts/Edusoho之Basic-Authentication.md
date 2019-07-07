---
title: Edusoho之Basic Authentication
date: 2019-07-01 19:45:18
tags: "Edusoho"
---

通过如下代码，可以正常请求并获取对应的数据:
```
curl -X POST -H "Accept:application/vnd.edusoho.v2+json" -H "Authorization: Basic dGVzdDJlZHVvc2hvOjEyMzQ1Ng==" http://demo.edusoho.com/api/tokens

```

但是我想将其中的请求地址替换成我自己的域名，却不行。

由于Edusoho开发文档并未对其详说，于是我便提相关的issue。

我提的issue如下:
https://github.com/edusoho/edusoho/issues/65
<!--more-->

最后这个问题，还是解决了。本次问题的原因还是我对于Http Basic Authentication(又称HTTP基本认证)缺乏了解，不然早就可以解决了。

本次解决主要参考这篇文章[HTTP Basic Authentication认证](https://www.cnblogs.com/yuqiangli0616/p/9389273.html)

这篇文章让我知道Authorization对于的值到底是什么意思。之前之所以得不到预想的结果就是因为不理解不知道。
当我知道dGVzdDJlZHVvc2hvOjEyMzQ1Ng，对应的实际上是用户名:密码(使用Base64加密))后，一切问题皆迎刃而解。

最后通过在线Base64加密http://tool.oschina.net/encrypt?type=3
将对应用户名:密码这种格式进行加密后成功，再次请求对应的接口，如图所示:

![图一](Edusoho之Basic-Authentication/01.png)

记住加密一定要按照如下格式：
格式=> 用户名:密码(中间加冒号)
![图二](Edusoho之Basic-Authentication/02.png)

请求成功获取token，拿到token后，这时你就可以玩玩其他的接口(建议对Edusoho二次开发的朋友们可以试试，非二次开发的朋友也可以借鉴它的这种接口鉴权方式，然后为其所用)。

Edusoho API地址为:http://developer.edusoho.com/api

关于HTTP常用的场景，如图所示(这种就比较常见了):
![图二](Edusoho之Basic-Authentication/03.png)

相信这种场景，大家都不陌生。

Http常用认证如下五种:

Http Basic Auth

OAuth

Cookie Auth

Token Auth

基于JWT的Token认证机制


认证的根本目的就是让用户访问更加安全，防止不法分子窃取用户名和密码做一些违法的事情。

关于上面的认证，后续用到后，会有一个详解。

顺便补充一点，实际开发系统并不建议使用Http Basic Auth这种方式，因为它简单(每次请求API都提供用户的username和password)，参数中携带如此敏感信息，有着极大的暴露风险，所以在生产环境中被使用的越来越少。

参考资料如下:
[HTTP 几种常用的认证机制](https://blog.csdn.net/ctwy291314/article/details/83275456)
[HTTP Basic Authentication认证](https://www.cnblogs.com/yuqiangli0616/p/9389273.html)