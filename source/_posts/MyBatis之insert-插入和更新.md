---
title: MyBatis之insert(插入和更新)
date: 2021-04-03 18:01:58
tags: "MyBatis"
---
近来在改造一个同事的数据入库项目，发现了一些问题，其中就因为数据库联合主键的缘故导入新增的数据不能入库(这个新增的入库数据，其实对应的数据表就存在这样的数据，她那边没有针对此进行判断有则更新，仅仅是无则插入)。
基于这个问题，我不想写太多的代码(查这条数据是否存在，存在则更新这样的)，只想用最少的代码量解决这个问题，通过搜索我找到了这样的方法，无需写很多代码，就是一条SQL就能搞定。
<!--more-->

## 1.针对单条插入或更新而言，SQL可以这样写(对应的XML里面的SQL)
以我博客为例:
```
 INSERT INTO `wp_users` (
                    `ID`,
                    `user_login`,
                    `user_pass`,
                    `user_nicename`,
                    `user_email`,
                    `user_url`,
                    `user_registered`,
                    `user_activation_key`,
                    `user_status`
                    `display_name`
                    )
            VALUES
                (
                #{ID},
                #{user_login},
                #{user_pass},
                #{user_nicename},
                #{user_email},
                #{user_url},
                #{user_registered},
                #{user_activation_key},
                #{user_status},
                #{display_name}
                )
                    ON DUPLICATE KEY UPDATE
        ID = VALUES(ID),
        user_login = VALUES(user_login),
        user_pass = VALUES(user_pass),
        user_nicename = VALUES (user_nicename),
        user_email = VALUES (user_email),
        user_url = VALUES (user_url),
        user_registered = VALUES (user_registered),
        user_activation_key = VALUES (user_activation_key),
        user_status = VALUES (user_status),
        display_name = VALUES (display_name)
```

## 2.针对批量插入或更新而言可以这么写(对应的XML里面的SQL)
以我博客为例:
```
INSERT INTO `wp_users` (
                    `ID`,
                    `user_login`,
                    `user_pass`,
                    `user_nicename`,
                    `user_email`,
                    `user_url`,
                    `user_registered`,
                    `user_activation_key`,
                    `user_status`
                    `display_name`
                    )
            VALUES
            
	<foreach collection="list" item="item" index="index" separator=",">

                (
                #{item.ID},
                #{item.user_login},
                #{item.user_pass},
                #{item.user_nicename},
                #{item.user_email},
                #{item.user_url},
                #{item.user_registered},
                #{item.user_activation_key},
                #{item.user_status},
                #{item.display_name}
                )
	</foreach>
                    ON DUPLICATE KEY UPDATE
        ID = VALUES(ID),
        user_login = VALUES(user_login),
        user_pass = VALUES(user_pass),
        user_nicename = VALUES (user_nicename),
        user_email = VALUES (user_email),
        user_url = VALUES (user_url),
        user_registered = VALUES (user_registered),
        user_activation_key = VALUES (user_activation_key),
        user_status = VALUES (user_status),
        display_name = VALUES (display_name)
```
参考资料如下:
[mybatis对mysql进行插入或者更新(含批量)](https://blog.csdn.net/AlbenXie/article/details/109641690)