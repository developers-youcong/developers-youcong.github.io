---
title: mysql查询结果多列拼接查询
date: 2020-02-08 00:06:42
tags: "MySQL"
---

mysql查询结果多列拼接查询,主要场景是，列表中其中一列涉及另外一张表的多条数据，但是我只需要多条数据中的其中某一列(主子表场景)

关键字:GROUP_CONCAT

sql语句如下:
```
SELECT
            r.id,b.NAME AS group_name,GROUP_CONCAT( a.`name` ) province_name
            FROM config_rule AS r
            LEFT JOIN group AS g ON ( r.svg_id = g.id )
            LEFT JOIN config_rule_detail AS rd ON ( r.id = rd.chat_config_rule_id )
            LEFT JOIN area AS a ON ( rd.province_id = a.id )
            WHERE r.company_id = 1 AND r.delete_flag = 'N' AND rd.delete_flag = 'N' and r.chat_mongo_id = '123456'

```

参考地址:https://blog.csdn.net/qq_35548288/article/details/81771978