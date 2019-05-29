---
title: Edusoho之LNMP环境搭建
date: 2019-04-10 16:55:14
tags: "Linux"
---


## 1.更新
```
sudo apt-get update
sudo apt-get upgrade

```

## 2.安装Nginx
```
sudo apt-get install nginx

```

## 3.安装php
```
sudo apt-get install php-pear php7.0-cli php7.0-common php7.0-curl \
    php7.0-dev php7.0-fpm php7.0-json php7.0-mbstring php7.0-mcrypt \
    php7.0-mysql php7.0-opcache php7.0-zip php7.0-intl php7.0-gd php7.0-xml

```
<!--more-->
修改配置
```
sudo vim /etc/php/7.0/fpm/php.ini

```

修改配置内容如下:
```
post_max_size = 1024M
memory_limit = 1024M
upload_max_filesize = 1024M

```

重启让配置生效:
```
sudo service php7.0-fpm restart

```

## 4.安装mysql
```
sudo apt-get update
sudo apt-get install mysql-server

```

## 5.安装edusoho
```
cd /var/www
sudo wget http://download.edusoho.com/edusoho-8.3.20.tar.gz 
sudo tar -zxvf edusoho-8.3.20.tar.gz 
sudo chown www-data:www-data edusoho/ -Rf

```

## 6.修改配置文件
```
sudo vim /etc/nginx/sites-enabled/edusoho

```

edusoho配置如下:
```
server {
    listen 80;

    # [改] 域名或IP
    server_name 192.168.126.130;
    
    #301跳转可以在nginx中配置

    # 程序的安装路径
    root /var/www/edusoho/web;

    # 日志路径
    access_log /var/log/nginx/example.com.access.log;
    error_log /var/log/nginx/example.com.error.log;

    location / {
        index app.php;
        try_files $uri @rewriteapp;
    }

    location @rewriteapp {
        rewrite ^(.*)$ /app.php/$1 last;
    }
     location ~ ^/udisk {
        internal;
        root /var/www/edusoho/app/data/;
    }
    location ~ ^/(app|app_dev)\.php(/|$) {
        fastcgi_pass   unix:/var/run/php7.0-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_param  HTTPS              off;
        fastcgi_param HTTP_X-Sendfile-Type X-Accel-Redirect;
        fastcgi_param HTTP_X-Accel-Mapping /udisk=/var/www/edusoho/app/data/udisk;
        fastcgi_buffer_size 128k;
        fastcgi_buffers 8 128k;
    }

    # 配置设置图片格式文件
    location ~* \.(jpg|jpeg|gif|png|ico|swf)$ {
        # 过期时间为3年
        expires 3y;
        
        # 关闭日志记录
        access_log off;

        # 关闭gzip压缩，减少CPU消耗，因为图片的压缩率不高。
        gzip off;
    }

    # 配置css/js文件
    location ~* \.(css|js)$ {
        access_log off;
        expires 3y;
    }

    # 禁止用户上传目录下所有.php文件的访问，提高安全性
    location ~ ^/files/.*\.(php|php7.0)$ {
        deny all;
    }
 # 以下配置允许运行.php的程序，方便于其他第三方系统的集成。
    location ~ \.php$ {
        # [改] 请根据实际php-fpm运行的方式修改
        fastcgi_pass   unix:/var/run/php7.0-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_param  HTTPS              off;
        fastcgi_param  HTTP_PROXY         "";
    }
}
```

其实LNMP环境搭建与LAMP环境搭建基本相同，只不过使用的web服务器不同及其配置有点差异，一句话，大同小异。
