---
title: PostgreSQL安装和使用
date: 2019-03-16 19:41:45
tags: "PostgreSQL"
---

青岛OJ系统用的关系型数据库是PostgreSQL,为此对PostgreSQL大致了解下。

今天的主要话题围绕下面两个方面:

- PostgreSQL安装
- PostgreSQL使用

<!--more-->
## 一、PostgreSQL安装(以Ubuntu16.04为例)

#### 1.安装命令
```
apt install postgresql

```

#### 2.修改 /etc/postgresql/9.5/main/postgresql.conf
```
将下面 listen_addresses = 'localhost'注释去掉并改为  listen_addresses = '*'
# - Connection Settings -
#listen_addresses = 'localhost'          
# what IP address(es) to listen on;                
# comma-separated list of addresses;     
# defaults to 'localhost'; use '*' for all
...

将下面password_encryption = on 注释打开
#password_encryption = on

```

#### 3.切换用户su - postgres

#### 4.通过psql 命令进入postgresql客户端

#### 5.修改用户密码
```
ALTER USER postgres PASSWORD 'youcongtech';
```

#### 6.vim /etc/postgresql/9.5/main/pg_hba.conf 修改 host all all 192.168.1.0/24 md5 中的ip,为：0.0.0.0/0

```
# TYPE DATABASE  USER    CIDR-ADDRESS     METHOD
# "local" is for Unix domain socket connections only
local all    all               trust
# IPv4 local connections:
host  all    all    127.0.0.1/32     trust
host  all    all    192.168.126.0/24    md5
# IPv6 local connections:
host  all    all    ::1/128       trust

```

windows安装PostgreSQL:https://www.cnblogs.com/sharpest/p/6225028.html

## 二、PostgreSQL使用

PostgreSQL官方网站:https://www.postgresql.org/
PostgreSQL中文教程:https://www.yiibai.com/postgresql/

#### 1.介绍PostgreSQL
PostgreSQL是一个功能强大的开源对象关系数据库系统，它使用和扩展了SQL语言，并结合了许多安全存储和扩展最复杂数据工作负载的功能。PostgreSQL的起源可以追溯到1986年，作为加州大学伯克利分校POSTGRES项目的一部分，并在核心平台上进行30多年的积极开发。
PostgreSQL凭借其经过验证的架构，可靠性、数据完整性，强大的功能集，可扩展性以及软件背后的开源社区的奉献精神赢得了良好的声誉，以始终如一地提供高性能和创新的解决方案。PostgreSQL在所有主要操作系统上运行，自2001年以来一直是符合ACID标准的，并且具有强大的附加功能，例如流行的PostGIS地址空间数据库扩展器。毫无疑问，PostgerSQL已成为许多人和组织首选的开源关系数据库。


#### 为什么要使用PostgreSQL
PostgreSQL提供了许多功能，旨在帮助开发人员构建应用程序，管理员保护数据完整性并构建容错环境，并帮助您管理数据，无论数据集有多大或多小。除了免费和开源之外，PostgeSQL还具有高度可扩展性。例如，您可以定义自己的数据类型，构建自定义函数，甚至可以编写来自不同编程语言的代码，而无需重新编译数据库。
PostgreSQL试图符合SQL标准，在这种标准中，这种一致性不会与传统特性相矛盾，或者可能导致糟糕的架构决策。支持SQL标准所需的许多功能，但有时语法或功能略有不同。随着时间的推移，可以预期进一步向一致性迈进。从2018年10月发布的版本11开始，PostgreSQL符合SQL:2011核心一致性的179个强制性功能中的至少160个，在撰写本文时，没有任何关系数据库符合此标准的完全符合性。

下面是PostgreSQL中各种功能的无穷无尽的功能，每个主要版本都添加更多功能:
##### 数据类型
基元：整数，数字，字符串，布尔值
结构化：日期/时间，数组，范围，UUID
文档：JSON / JSONB，XML，键值（Hstore）
几何：点，线，圆，多边形
自定义：复合，自定义类型

##### 数据的完整性
独一无二，不是空的
主键
外键
排除约束
显式锁定，咨询锁定
##### 并发性，性能
索引：B树，多列，表达式，部分
高级索引：GiST，SP-Gist，KNN Gist，GIN，BRIN，覆盖索引，布隆过滤器
复杂的查询计划器/优化器，仅索引扫描，多列统计
交易，嵌套交易（通过保存点）
多版本并发控制（MVCC）
读取查询的并行化和构建B树索引
表分区
SQL标准中定义的所有事务隔离级别，包括Serializable
即时（JIT）表达式汇编

##### 可靠性，灾难恢复
预写日志（WAL）
复制：异步，同步，逻辑
时间点恢复（PITR），主动备用
表空间
##### 安全
身份验证：GSSAPI，SSPI，LDAP，SCRAM-SHA-256，证书等
强大的访问控制系统
列和行级安全性

##### 可扩展性
存储的功能和程序
程序语言：PL / PGSQL，Perl，Python（以及更多）
外部数据包装器：使用标准SQL接口连接到其他数据库或流
许多提供附加功能的扩展，包括PostGIS

##### 国际化，文本搜索
支持国际字符集，例如通过ICU校对
全文检索
您可以在PostgreSQL 文档中发现更多功能。此外，PostgreSQL具有高度可扩展性：许多功能（如索引）都定义了API，因此您可以使用PostgreSQL构建以解决您的挑战。

事实证明，PostgreSQL在可管理的大量数据和可容纳的并发用户数量方面具有高度可扩展性。生产环境中有活跃的PostgreSQL集群可管理数TB的数据，以及管理PB级的专用系统。


本文参考资料:
[Ubuntu 16.04 安装使用PostgreSQL最佳指南](https://www.jianshu.com/p/dda94c4ffd52)
[PostgreSQL官网](https://www.postgresql.org/)


