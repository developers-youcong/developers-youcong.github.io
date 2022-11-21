---
title: Nginx之跨域、限制连接、限制下载速度
date: 2022-03-15 21:52:16
tags: "Nginx"
---

此次分享的主要内容如下:

- 1.Nginx跨域配置；
- 2.Nginx限制连接配置；
- 3.Nginx限制下载速度配置。
<!--more-->

## 一、Nginx跨域配置
```
#允许跨域请求的域，* 代表所有
add_header 'Access-Control-Allow-Origin' *;
#允许请求的header
add_header 'Access-Control-Allow-Headers' *;
#允许带上cookie请求
add_header 'Access-Control-Allow-Credentials' 'true';
#允许请求的方法，比如 GET,POST,PUT,DELETE
add_header 'Access-Control-Allow-Methods' *;

```

## 二、Nginx限制连接配置
```
 location / {
	root   /var/www/test;
	index  index.php index.html index.htm;
	limit_conn addr 5; #是限制每个IP只能发起5个连接
}


```

## 三、Nginx限制下载速度配置
```
location /download { 
       limit_rate_after 10m; 
       limit_rate 128k; 
 }  

```
