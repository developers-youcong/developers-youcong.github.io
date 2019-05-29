---
title: Docker安装MySQL并配置远程访问
date: 2019-03-27 16:36:03
tags: "Docker"
---

## 1.docker search mysql   查看mysql版本

## 2.docker pull mysql  要选择starts最高的那个name 进行下载

## 3.docker images  查看下载好的镜像

## 4.启动mysql实例
 docker run --name dockermysql  -p 3307:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql

    --name 为mysql的实例设置别名。 -p 3307为对外暴露的端口。3306是内部端口 

    -e MYSQL_ROOT_PASSWORD 设置mysql登录密码  -d 以守护进程运行（后台运行） 最后的mysql是镜像名称
## 5. docker ps -a 查看在运行的

## 6. docker exec -it dockermysql bash     进入容器内部  dockermysql 是上边运行时为容器取的别名 也可以用id替代
 另外进入容器后，你如果想要使用vim或vi编辑文件，请先执行apt install vim安装对应的库，否则会出现command not found这样的错误提示
 
 
## 7.mysql -u root -p      然后直接输入密码即可 密码是在运行时设置的

## 8.grant all privileges on *.*  to 'root'@'%' ;   给用于授予权限

 GRANT ALL PRIVILEGES ON *.*  ‘root’@’%’ identified by ‘123123’ WITH GRANT OPTION;  这是网上流传较多的写法。实际上会报错的(本人试验了确实报错，比如像mysql5.7以下的通常是好使的，像现在比较高的版本就不好使了,比如我这个版本是8.0.15)

<!--more-->
## 9.flush privileges;  刷新权限


遇见一个错误mysql 1251

解决办法按照如下做法即可解决，然后就可以利用mysql客户端(如sqlyog或其它工具)
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #修改加密规则 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; #更新一下用户的密码 
FLUSH PRIVILEGES; #刷新权限 

```
'root'   为你自己定义的用户名

'localhost' 指的是用户开放的IP，可以是'localhost'(仅本机访问，相当于127.0.0.1)，可以是具体的'*.*.*.*'(具体某一IP)，也可以是 '%' (所有IP均可访问)

'password' 是你想使用的用户密码

参考资料:
docker部署mysql 实现远程连接:https://blog.csdn.net/weixin_42459563/article/details/80924634
Navicat连接Mysql8.0.11出现1251错误:https://blog.csdn.net/qq_36068954/article/details/80175755
