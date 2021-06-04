---
title: Openrestry+Lua之静态文件鉴权
date: 2021-01-04 19:36:34
tags: "Linux"
---

核心代码:
```
location /statis-img {
              alias /home/user/files/;
              access_by_lua '

                local token  = ngx.var.arg_token

                local access_token = "123456"

                if token == access_tokein then
                    return true
                else
                   ngx.exit(403)
                end
            ';

  }

```

这段代码的主要功能就是鉴权，/home/user/files下的静态文件访问是需要携带token的，携带token以及token正确的前提下才能访问到静态资源，否则被拦截并定向到403。
<!--more-->
Openrestry是Nginx的一种扩展增强，主要的一个体现就是通过编写lua脚本实现非常多的功能，如果要进行应用场景归类，可分为如下几大类:
- Web应用；
- 接入网关；
- Web防火墙；
- 缓存服务器。

关于Openrestry的搭建，可参考我的这篇文章:
[OpenResty源码编译安装](https://developers-youcong.github.io/2020/12/12/OpenResty%E6%BA%90%E7%A0%81%E7%BC%96%E8%AF%91%E5%AE%89%E8%A3%85/)

关于lua语言的学习，可参考如下链接:
https://www.runoob.com/lua/lua-tutorial.html

https://www.w3cschool.cn/lua/lua-tutorial.html