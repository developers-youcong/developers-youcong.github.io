---
title: Http之413错误解决
date: 2021-01-13 19:44:13
tags: "Linux"
---

### 1.错误信息

```

error:RPC失败。HTTP 413 curl 22 the request URL returned error:413

fatal:远端意外挂断了。

fatal:远端意外挂断了。



```

<!--more-->



### 2.错误原因

原因在于Nginx配置文件中的client_max_body_size，client_max_body_size如果不设置的话，默认为1m。



### 3.解决办法

在nginx配置文件中(nginx.conf)添加如下代码:

```

        client_max_body_size 8m;

        client_body_buffer_size 128k;

        fastcgi_intercept_errors on;



```



我的完整配置如下:

```



        location / {



              client_max_body_size 8m;

              client_body_buffer_size 128k;

              fastcgi_intercept_errors on;

              proxy_pass   http://eqics-api;

        }



```



### 4.参考链接

[HTTP 413错误解决方法](https://blog.csdn.net/qq_33571718/article/details/80242556)