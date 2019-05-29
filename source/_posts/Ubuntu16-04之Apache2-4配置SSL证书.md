---
title: Ubuntu16.04之Apache2.4配置SSL证书
date: 2019-05-17 17:39:12
tags: "Linux"
---
具体步骤不是特别复杂，有些细枝末节我可能忽略了，不过参考我的这个教程，应该可以配置好的，如果朋友们有问题，可以留言给我。
参考资料如下:
[Linux + Apache2 环境下配置 https (腾讯云免费证书)](https://blog.csdn.net/m_zhurunfeng/article/details/79980320)

[Ubuntu系统Apache 2部署SSL证书](https://help.aliyun.com/document_detail/102450.html)

虽然说很多不记得了，但是有这么几点必须要提。

第一、去阿里云下载证书，通过winscp或者xftp上传文件到服务器上
第二、解压证书zip包，并将其放入某个文件夹下
第三、安装apache，并按照如下步骤:
<!--more-->
安装Apache、启动SSL模块、重写模块等:
```
sudo apt-get install apache2

a2enmod ssl

a2enmod rewrite

```

编辑default-ssl.conf
```
vim /etc/apache2/sites-available/default-ssl.conf

```

修改如下(记住该文件默认就有，将对应的crt、key等修改为阿里云证书或者腾讯云证书对应的,下面是我阿里云对应的):
```
                SSLCertificateFile /home/yc/apache_https/1854029_www.yc.com_public.crt
                SSLCertificateKeyFile /home/yc/apache_https/1854029_www.yc.com.key
                SSLCertificateChainFile /home/yc/apache_https/1854029_www.yc.com_chain.crt


```


建立软链接:
```
ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/001-ssl.conf

```

重启Apache:
```
/etc/init.d/apache2 restart

```

最后你发现访问你的域名https://yourdomain.com 出现apache的欢迎页就表示OK了

另外如果你发现访问不了的话，记得在default-ssl.conf配置文件添加如下，然后重启一下Apache即可:
```
               <Directory "/var/www/html">
                 Options FollowSymLinks ExecCGI
                AllowOverride All
                Order allow,deny
                Allow from all
                Require all granted
                 </Directory>


```


