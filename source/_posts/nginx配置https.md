---
title: nginx配置https
date: 2019-02-26 20:06:25
tags: "Nginx"
---


其实nginx配置也差不太多，虽然差不太多，但还是有区别的。
<!--more-->
假定你已经在阿里云完成了证书申请，接下来你就可以按照如下配置(主要是修改nginx.conf文件)
```
server {
        listen 443;
        server_name www.youcongtech.com;
        ssl on;
        index index.html;
        ssl_certificate /usr/local/nginx/cert/1854029_www.youcongtech.com.pem;
        ssl_certificate_key /usr/local/nginx/cert/1854029_www.youcongtech.com.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        location / {
                proxy_set_header X-Forwarded-Host $host;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_redirect off;
                expires off;
                sendfile off;
                proxy_pass http://www.youcongtech.com;
}

```

按照这样配置你不一定成功，可能会遇见下面的几个错误？

## 错误1

错误信息:ubuntu error: SSL modules require the OpenSSL library.

解决办法:
执行该命令即可解决:sudo apt-get install openssl libssl-dev
参考链接为:https://yq.aliyun.com/articles/486172

## 错误2

错误信息:nginx: [emerg] unknown directive "ssl" in /usr/local/nginx/conf/nginx.conf:188

解决办法:
在Nginx解压目录执行该命令:./configure --with-http_ssl_module

参考链接为:https://blog.csdn.net/weiyangdong/article/details/80008543

如果你对此还有疑问的话，可以参考这个链接:https://www.cnblogs.com/tianhei/p/7726505.html
这个链接会讲如何申请证书到配置https成功。

最后在浏览器输入:https://www.youcongtech.com 你会看到一个博客界面或者是一个Nginx欢迎页。
当然了，目前我的阿里云域名尚在认证中，过段时间就可以申请下来了。