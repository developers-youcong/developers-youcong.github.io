---
title: nginx之MySQL代理
date: 2021-01-22 21:27:58
tags: "Linux"
---

修改nginx.conf，添加如下配置(注意:stream和http是同级的):
<!--more-->
```
stream {
    upstream cloudsocket {
       hash $remote_addr consistent;
       server 192.168.1.101:3389 weight=5 max_fails=3 fail_timeout=30s;
    }
    server {
       listen 25674; # 数据库服务器监听端口
       proxy_connect_timeout 10s;
       proxy_timeout 300s; # 设置客户端和代理服务之间的超时时间，如果5分钟内没操作将自动断开。
       proxy_pass cloudsocket;
    }
}

```

配置stream的前提，该Nginx要按照如下命令编译安装才行，否则会报错:
```
./configure --prefix=/usr/local/nginx-mysql --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35 --with-stream


```
参考链接:
[nginx转发mysql连接](https://www.cnblogs.com/DreamFather/p/11358131.html)