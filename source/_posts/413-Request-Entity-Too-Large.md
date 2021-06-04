---
title: 413 Request Entity Too Large
date: 2021-01-22 22:55:34
tags: "Linux"
---

之前遇到413的错误信息如下:
```
error:RPC失败。HTTP 413 curl 22 the request URL returned error:413

fatal:远端意外挂断了。

fatal:远端意外挂断了。

```
这个错误是git推送代码，代码文件过多导致的。

之前的解决办法，可以参考如下:
[Http之413错误解决](https://developers-youcong.github.io/2021/01/13/Http%E4%B9%8B413%E9%94%99%E8%AF%AF%E8%A7%A3%E5%86%B3/)

今天遇到的这个错误关键核心信息是:
```
413 Request Entity Too Large

```

翻译过来，请求数据量过大。
<!--more-->
问题的原因基本一样，都与请求数据有关，解决办法自然一样，只不过表达形式不一样,只需在nginx代理配置如下，即可解决(其中client_max_box_size配置多大，与请求数据量有关):
```
client_max_body_size 20m;

client_body_buffer_size 128k;

fastcgi_intercept_errors on;


```
