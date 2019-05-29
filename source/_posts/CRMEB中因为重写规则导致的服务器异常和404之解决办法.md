---
title: CRMEB中因为重写规则导致的服务器异常和404之解决办法
date: 2019-05-21 13:52:35
tags: "Linux"
---
问题描述:
安装CRMEB后，只能通过https://域名//index.php/admin访问到后台，而不能直接通过https://域名/admin访问到后台，以至于导致进入系统后台出现有的功能界面可用，有的功能界面则出现404或者服务器异常之类的，从浏览器上看就是路径方面的原因导致的，实际原因则是apache没有开启重写模块导致的。
<!--more-->
问题解决方案:
第一、.htaccess(根目录有一个.htaccess，这个要与如下保持一致)
```
<IfModule mod_rewrite.c>
Options +FollowSymlinks -Multiviews
RewriteEngine on

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>

```

第二、开启rewrite重写
```
sudo a2enmod rewrite
/etc/init.d/apache2 restart
```

参考问题解决方案:
[apache开启rewrite重写](https://www.cnblogs.com/li-mei/p/5959217.html)