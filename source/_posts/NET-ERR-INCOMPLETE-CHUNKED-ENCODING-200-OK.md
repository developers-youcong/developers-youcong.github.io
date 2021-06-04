---
title: 'NET::ERR_INCOMPLETE_CHUNKED_ENCODING 200 (OK)'
date: 2020-10-16 20:02:58
tags: "Linux"
---

**错误信息:**
```
NET::ERR_INCOMPLETE_CHUNKED_ENCODING 200 (OK)

```
<!--more-->
**错误背景:**
微服务不通过统一的nginx端口访问，能够正常请求接口并获取对应的响应。
但是通过nginx的话，则出现请求通(也就是响应200)，但始终没有得到正确的响应，同时上述错误 NET::ERR_INCOMPLETE_CHUNKED_ENCODING 200 (OK)。


**解决办法:**
在nginx中的对应的反向代理配置如下内容，即可解决:
```
proxy_buffer_size 1024k;
proxy_buffers 16 1024k;
proxy_busy_buffers_size 2048k;
proxy_temp_file_write_size 2048k;

```
我的反向代理完整配置:
```
location /forecast {

	 proxy_pass http://127.0.0.1:9999/;
	 proxy_buffer_size 1024k;
	 proxy_buffers 16 1024k;
	 proxy_busy_buffers_size 2048k;
	 proxy_temp_file_write_size 2048k;

}

```

**错误原因分析:**
1.nginx配置缓存区设置过小
2.nginx的临时目录（/proxy_temp）过大或没有权限写入缓存文件
3.磁盘空间不足

经过验证是第一种原因(nginx配置缓冲区设置过小)

**问**:nginx的缓冲区作用是什么？

**答**:如果客户端到nginx速度快，nginx到服务器速度慢，没有缓冲区，一点点数据量就直接发到客户端，十分浪费性能。
有了缓冲区，积累到一定量，再传输到客户端，减少了Tcp请求。
相反，客户端到nginx速度慢，nginx到服务器速度快，没有缓冲区，
nginx到服务器的连接就会一直保持在那边，直到客户端接受完毕。
有了缓冲区，返回内容放到缓冲区后，nginx到服务器的连接就能断开了，客户端从缓冲区拉取即可。


参考解决办法:

[NET::ERR_INCOMPLETE_CHUNKED_ENCODING 200 (OK)解决办法](https://blog.csdn.net/willingtolove/article/details/103372199)

[缓冲区的作用](https://www.jianshu.com/p/c1559fd01828)