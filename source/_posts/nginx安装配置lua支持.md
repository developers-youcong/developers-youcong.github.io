---
title: nginx安装配置lua支持
date: 2020-12-02 21:41:44
tags: "Linux"
---

nginx安装很简单，配置lua相关的支持就需要额外的安装一些库和编译。
<!--more-->
## 一、准备环境
```
yum -y install lua*
wget https://luajit.org/download/LuaJIT-2.0.4.tar.gz
wget https://github.com/simpl/ngx_devel_kit/archive/v0.2.19.tar.gz
wget https://github.com/openresty/lua-nginx-module/archive/v0.10.13.tar.gz

```

## 二、解压ngx_devel_kit以及lua-nginx-module
```
tar xf v0.2.19.tar.gz
tar xf v0.10.13.tar.gz

```

## 三、编译安装LuaJIT，即Lua及时编译器
```
tar xf LuaJIT-2.0.4.tar.gz
cd LuaJIT-2.0.4/
make && make install

```

## 四、进入解压的nginx执行命令
```
./configure --prefix=/usr/software/nginx_lua --with-http_ssl_module --with-http_stub_status_module --with-http_dav_module --with-file-aio --with-http_dav_module --add-module=../ngx_devel_kit-0.2.19/ --add-module=../lua-nginx-module-0.10.13/

```

## 五、写个hello world测试
nginx.conf配置如下内容:
```
location /test_lua {
    default_type text/html;
    content_by_lua_block {
            ngx.say("Hello Lua!") 
    }
}

```

修改配置文件后，重启。

通过浏览器访问/test_lua，有hello world出现，说明配置lua成功。

本文参考资料如下:
[Nginx安装配置Lua支持](https://blog.csdn.net/qq_31725371/article/details/85226116?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control)