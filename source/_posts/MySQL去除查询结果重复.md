---
title: MySQL去除查询结果重复
date: 2019-03-29 21:29:36
tags: "MySQL"
---

出现结果重复数SQL(四表关联):
```
		SELECT
		COUNT(post.ID )
		FROM wp_posts AS post LEFT JOIN
		wp_term_relationships AS relation
		ON(post.menu_order =
		relation.term_order) LEFT JOIN wp_term_taxonomy
		AS taxonomy
		ON(relation.term_taxonomy_id = taxonomy.term_id) LEFT JOIN
		wp_terms AS
		term ON(taxonomy.term_id = term.term_id)

```

正常的结果应该显示490条数据，但是结果显示了224941。

<!--more-->
解决这个办法是在对应的COUNT()里面加上DISTINCT

DISTINCT这个关键字主要用于过滤掉多余的重复记录只保留一条，但往往只用它来返回不重复记录的条数，而不是用它来返回不重记录的所有值。

注意:它有局限性，比如吧不能对应多个目标字段，只能对应一个目标字段。

解决重复结果书的SQL如下:
```
		SELECT
		COUNT(DISTINCT post.ID)
		FROM wp_posts AS post LEFT JOIN
		wp_term_relationships AS relation
		ON(post.menu_order =
		relation.term_order) LEFT JOIN wp_term_taxonomy
		AS taxonomy
		ON(relation.term_taxonomy_id = taxonomy.term_id) LEFT JOIN
		wp_terms AS
		term ON(taxonomy.term_id = term.term_id)

```

参考资料:
MySQL中count函数使用方法详解:https://blog.csdn.net/qq_31135027/article/details/79858184
