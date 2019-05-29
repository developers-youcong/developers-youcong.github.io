---
title: nginx让所有的http地址重定向到https
date: 2019-03-01 20:28:22
tags: "Nginx"
---
问:为什么让所有的http都重定向到https呢？
答:因为这样会使网站更安全些。
<!--more-->
那么我是如何在nginx配置，让输入http://www.youcongtech.com或者youcongtech.com全部都重定向到https://www.youcongtech.com的呢？
其实我仅仅只是在nginx.conf配置文件中的server配置了如下:
```
rewrite ^(.*)$  https://$host$1 permanent;

```

这段配置的含义将所有的http请求通过rewrite重写到https上。

下面贴一下我的nginx.conf配置文件(主要重要的):
```
 upstream  www.youcongtech.com{
         server   39.107.110.227:2019;

    }
 server {
        listen       80;
        server_name  www.youcongtech.com;
        rewrite ^(.*)$  https://$host$1 permanent;
        #charset koi8-r;

        #access_log  logs/host.access.log  main;


        location ~ / {
            root /usr/local/nginx/html;
            index index.html index.htm;

        }
      error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }


      server {
        listen 443;
        server_name www.youcongtech.com;
        ssl on;
        index index.html;
        ssl_certificate /usr/local/nginx/cert/18540291_www.youcongtech.com.pem;
        ssl_certificate_key /usr/local/nginx/cert/18540291_www.youcongtech.com.key;
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
                root /usr/local/nginx/html;
                index index.html index.htm;
        }

    	location ~ ^/blog/(.*){
                 proxy_set_header Host $host;
                 proxy_set_header X-Real-IP $remote_addr;
                 proxy_pass http://www.youcongtech.com;    #转向tomcat处理

         }


}

```
上面参数到底是什么意思，加或者不加到底会怎么样，关于nginx参数详解和更好的优化，后续会有详细的讲解，我会继续编写我的博客系统，并以此作为案例。
当然了，如果公司涉及这块比较多，后续我也会以公司案例来给大家讲解。


本文主要参考该地址:https://www.cnblogs.com/kevingrace/p/6187072.html



