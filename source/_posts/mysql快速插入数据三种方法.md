---
title: mysql快速插入数据三种方法
date: 2021-01-18 21:34:52
tags: "MySQL"
---
今日发现一个独立的api微服务插入数据过慢，主要体现在日志aop的数据入库。于是我通过搜索想知道如何提高mysql数据库插入数据的效率。通过搜索我找到了三种方法:
- (1)修改mysql配置文件(mysql的ini文件增加bulk_insert_buffer_size=100M);
- (2)改写insert语句(使用insert delayed into);
- (3)一次插入多条数据(使用insert into table values('张三','18'),('李四','22'),('王五','28')...;)。

其中我尝试了第二种方法，效果能直接看到，只不过有延迟，没有普通insert into那样实时性(从字面上就很好理解，即延迟插入)，
这种方法和我用线程池异步处理效果很相似。

参考资料如下:
[mysql千万级数据库插入速度和读取速度的调整记录](https://www.cnblogs.com/wangsongbai/p/10493700.html)




