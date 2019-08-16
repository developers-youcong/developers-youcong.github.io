---
title: 为 Nginx 添加 HTTP 基本认证(HTTP Basic Authentication)
date: 2019-07-22 11:01:11
tags: "Nginx"
---
针对sudo apt-get install命令安装的nginx(默认会有/etc/nginx/conf.d)
```
sudo apt-get install nginx

```

## 生成密码
```
printf "your_username:$(openssl passwd -crypt your_password)\n" >> conf.d/passwd

```
如果没有conf.d/passwd这个文件，就自行创建
<!--more-->

## 配置nginx
修改配置文件:
```
vim /etc/nginx/sites-enabled/default

```

修改内容如下:
```
location /
{
    auth_basic "网站名称";
    auth_basic_user_file conf.d/passwd; 
    autoindex on;
}

```

## 重启nginx
```
/etc/init.d/nginx restart

```


## 访问效果如下
![](为-Nginx-添加-HTTP-基本认证-HTTP-Basic-Authentication/01.png)


