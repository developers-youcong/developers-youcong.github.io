---
title: Ubuntu16.04Apache负载均衡+集群
date: 2019-05-28 10:42:40
tags: "Linux"
---
mod_proxy ，主代理模块Apache模块用于重定向连接;它允许Apache充当底层应用程序服务器的网关。
mod_proxy_http ，它增加了对代理HTTP连接的支持。
mod_proxy_balancer和mod_lbmethod_byrequests ，它为多个后端服务器添加负载平衡功能。

为了保证配置流程正常，请执行如下命令:
```
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod lbmethod_byrequests
/etc/init.d/apache2 restart

```
<!--more-->

编辑该配置文件(vim /etc/apache2/sites-available/000-default.conf)，添加如下:
```
<VirtualHost *:80>
    ProxyPreserveHost On

    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/
</VirtualHost>

```
这样就可以访问了，但是如果是多台服务器的话，添加如下配置:
```
<VirtualHost *:80>
<Proxy balancer://mycluster>
    BalancerMember http://127.0.0.1:8080
    BalancerMember http://127.0.0.1:8081
</Proxy>

    ProxyPreserveHost On

    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/
</VirtualHost>

```
换言之如果是https请求，配置也是一样的，不一样的是文件不同(如果是配置https，需要修改/etc/apache2/sites-available/default-ssl.conf)
内容与上面一样，唯一不一样的是端口，SSL默认是443端口。

如果你不知道apache如何配置https,可以参考我的这篇博客[Ubuntu16.04之Apache2.4配置SSL证书](https://developers-youcong.github.io/2019/05/17/Ubuntu16-04%E4%B9%8BApache2-4%E9%85%8D%E7%BD%AESSL%E8%AF%81%E4%B9%A6/)

本文参考链接资料如下:
[如何在Ubuntu 16.04上使用Apache的mod_proxy作为反向代理](https://www.howtoing.com/how-to-use-apache-as-a-reverse-proxy-with-mod-proxy-on-ubuntu-16-04)