---
title: 《高性能MySQL》之EXPLAIN
date: 2020-02-02 14:38:03
tags: "MySQL"
---
## 使用explain关键字获取sql执行性能
<!--more-->
语法如下:
```
explain select * from table
```
explain 中的列
expain出来的信息有10列，
分别是id,select_type,table、type,partitions,possible_keys,key,key_len,ref,rows,Extra,下面对这些字段出现的可能进行解释：

### 1.ID
SQL执行的顺序的标识,SQL从大到小的执行

ID相同时，执行顺序由上至下
如果是子查询，ID的序号会递增，ID值越大优先级越高，越先被执行
ID如果相同，可以认为是一组，从上往下顺序执行；在所有组中，ID值越大，优先级越高，越先执行。


### 2.select_type
示查询中每个select子句的类型

SIMPLE：简单的SELECT，不实用UNION或者子查询。

PRIMARY：最外层SELECT。

UNION：第二层，在SELECT之后使用了UNION。

DEPENDENT UNION：UNION语句中的第二个SELECT，依赖于外部子查询。

UNION RESULT：UNION的结果。

SUBQUERY：子查询中的第一个SELECT。

DEPENDENT SUBQUERY：子查询中的第一个SELECT，取决于外面的查询。

DERIVED：导出表的SELECT（FROM子句的子查询）

MATERIALIZED：物化子查询

UNCACHEABLE SUBQUERY：无法缓存结果的子查询，必须为外部查询的每一行重新计算

UNCACHEABLE UNION：UNION 属于不可缓存的子查询的第二个或后一个选择

### 3.table
输出行引用的表的名称。这也可以是以下值之一：

<unionM,N,...>：该行指的是id值为M和id值为N的并集。
<derivedN>：该行是指用于与该行的派生表结果id的值 N。例如，派生表可以来自FROM子句中的子查询
<subqueryN>：该行指的是id 值为的行的具体化子查询的结果N


### 4.type
表示MySQL在表中找到所需行的方式，又称“访问类型”。

常用的类型有： NULL, system, const, eq_ref, ref, range, index, ALL（从左到右，性能从差到好）
以下列表描述了从最佳类型到最差类型的连接类型

#### (1)NULL
MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成。

#### (2)system
该表只有一行（如：系统表）。这是const连接类型的特例

#### (3)const
该表最多只有一个匹配行，在查询开头读取。因为只有一行，所以优化器的其余部分可以将此行中列的值视为常量。 const表非常快，因为它们只读一次。

获取单个文章详细信息:
```
EXPLAIN SELECT post_title,post_content,post_date FROM wp_posts WHERE ID = 1
```

查询指定分类所有文章:

```

EXPLAIN SELECT u.`user_nicename`,p.`ID`,p.`post_title`,p.`post_content`,p.`post_date`,p.`post_excerpt`,term.`name`,relation.`term_taxonomy_id`,term.`name` FROM wp_users AS u 
LEFT JOIN wp_posts p ON(u.`ID` = p.`post_author`) 
LEFT JOIN wp_term_relationships AS relation ON(p.`ID` = relation.`object_id`)
LEFT JOIN wp_term_taxonomy AS taxonomy ON(relation.`term_taxonomy_id` = taxonomy.`term_taxonomy_id`)
LEFT JOIN wp_terms AS term ON(taxonomy.`term_id` = term.`term_id`)
WHERE relation.`term_taxonomy_id` = 2
ORDER BY p.`post_date`

```


#### (4)eq_ref
对于前面表格中的每个行组合，从该表中读取一行。除了 system和 const类型之外，这是最好的连接类型。当连接使用索引的所有部分且索引是 索引PRIMARY KEY或UNIQUE NOT NULL索引时使用它。

获取所有文章分类:
```
 SELECT
		term.`term_id`,term.`name` FROM wp_term_taxonomy AS taxonomy LEFT JOIN
		wp_terms AS term ON(term.`term_id` = taxonomy.`term_id`)
		WHERE
		taxonomy.`taxonomy` = 'category'
```


#### (5)ref
表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值。

获取指定用户所有文章

```
EXPLAIN SELECT
u.display_name,u.user_nicename,p.ID,p.post_title,p.post_content,p.post_date,p.post_modified,p.post_date,p.post_status,p.comment_count
FROM wp_users AS u INNER JOIN wp_posts AS p ON(u.ID=p.post_author) 
WHERE p.post_status = "publish"	
AND u.user_nicename = 'youcongtech'
ORDER BY p.post_modified DESC LIMIT 0,10
```


```
EXPLAIN SELECT p.`ID`,p.`post_title`,p.`post_content` FROM wp_posts AS p  WHERE p.`post_author` IN(SELECT u.`ID` FROM wp_users AS u WHERE u.`ID` = 1); 
```




#### (6)fulltext
使用FULLTEXT 索引执行连接。


添加FULLTEXT:
```
ALTER TABLE wp_posts ADD FULLTEXT INDEX ngram_idx(post_title) WITH PARSER ngram;
```



全文检索:
```
 EXPLAIN SELECT * FROM wp_posts WHERE MATCH(post_title) AGAINST('HustOJ%' IN BOOLEAN MODE);
```


#### (7)ref_or_null

```
SELECT * FROM ref_table WHERE key_column IS NULL;
```

#### (8)range
仅检索给定范围内的行，使用索引选择行的key 输出行中的列指示使用哪个索引。将key_len包含已使用的时间最长的关键部分。该ref列 NULL适用于此类型。
range当一个键柱使用任何的相比于恒定可使用 =， <>， >， >=， <， <=， IS NULL， <=>， BETWEEN， LIKE，或 IN()：

根据作者ID查询指定范围内的文章:
```
EXPLAIN SELECT * FROM wp_posts AS p WHERE p.`post_author` = 1 AND p.`ID` IN(1,2)
```


#### (9)index
该index联接类型是一样的 ALL，只是索引树被扫描。这种情况有两种：

a.如果索引是查询的覆盖索引，并且可用于满足表中所需的所有数据，则仅扫描索引树。在这种情况下，Extra专栏说 Using index。仅索引扫描通常比ALL索引的大小通常小于表数据更快 。

b.使用索引中的读取执行全表扫描，以按索引顺序查找数据行。 Uses index没有出现在 Extra列中。当查询仅使用属于单个索引的列时，MySQL可以使用此连接类型。

获取某一段时间发布的文章总数:

```
EXPLAIN SELECT COUNT(p.`ID`) FROM wp_posts AS p WHERE p.post_status = 'publish' AND p.`post_date` BETWEEN '2019-08-11 00:00:00' AND '2019-12-15 23:59:59'
```


#### (10)ALL
对前面表格中的每个行组合进行全表扫描。如果表是第一个未标记的表 const，通常不好，并且在所有其他情况下通常 非常糟糕。通常，您可以ALL通过添加基于常量值或早期表中的列值从表中启用行检索的索引来避免。



```
EXPLAIN SELECT p.`post_title`,p.`post_content` FROM wp_posts AS p

```


### 5.possible_keys
该possible_keys列指示MySQL可以选择在此表中查找行的索引，指出MySQL能使用哪个索引在表中找到记录，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用

该列完全独立于EXPLAIN输出所示的表的次序。这意味着在possible_keys中的某些键实际上不能按生成的表次序使用。
如果该列是NULL，则没有相关的索引。在这种情况下，可以通过检查WHERE子句看是否它引用某些列或适合索引的列来提高你的查询性能。如果是这样，创造一个适当的索引并且再次用EXPLAIN检查查询

### 6.Key
key列显示MySQL实际决定使用的键（索引）

如果没有选择索引，键是NULL。要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。

### 7.key_len
表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度（key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的）

不损失精确性的情况下，长度越短越好

### 8.ref
表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值

### 9.rows
表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数

### 10.Extra
该Extra列 EXPLAIN输出包含MySQL解决查询的额外信息。以下列表说明了此列中可能出现的值。每个项目还指示JSON格式的输出哪个属性显示Extra值。对于其中一些，有一个特定的属性。其他显示为message 属性的文本

### 11.partitions（扩展）
记录将与查询匹配的分区。仅在使用PARTITIONS关键字时才显示此列 。非分区表显示null

参考资料链接:
https://segmentfault.com/a/1190000017751405