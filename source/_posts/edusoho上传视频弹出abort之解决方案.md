---
title: edusoho上传视频弹出abort之解决方案
date: 2019-06-12 18:03:02
tags: "Linux"
---
错误描述:edusoho上传如avi、mp4等容量大的图片(如100m以上或500m等)弹出abort提示框

原因:是因为web服务器apache默认上传文件有限制导致的

解决办法如下:
<!--more-->
(1)首先修改改php.ini配置文件
`sudo vim /etc/php/7.0/fpm/php.ini`

并修改如下内容：
```
post_max_size = 1024M
memory_limit = 1024M
upload_max_filesize = 1024M

```

(2)不仅仅需要修改这个，还需修改vim /etc/php/7.0/apache2/php.ini
修改内容同上所示
```
post_max_size = 1024M
memory_limit = 1024M
upload_max_filesize = 1024M


```

(3)修改完后记得重启php和apache
```
/etc/init.d/php7.0-fpm restart
/etc/init.d/apache2 restart

```