---
title: 从零开始学YC-Framework之PostgreSQL
date: 2022-09-24 20:10:37
tags: "YC-Framework"
---

## 一、PostgreSQL是什么？
<!--more-->
ostgreSQL是一个功能强大的开源对象关系数据库系统，它使用和扩展了SQL语言，并结合了许多安全存储和扩展最复杂数据工作负载的功能。PostgreSQL的起源可以追溯到1986年，作为加州大学伯克利分校POSTGRES项目的一部分，并在核心平台上进行30多年的积极开发。
PostgreSQL凭借其经过验证的架构，可靠性、数据完整性，强大的功能集，可扩展性以及软件背后的开源社区的奉献精神赢得了良好的声誉，以始终如一地提供高性能和创新的解决方案。PostgreSQL在所有主要操作系统上运行，自2001年以来一直是符合ACID标准的，并且具有强大的附加功能，例如流行的PostGIS地址空间数据库扩展器。毫无疑问，PostgerSQL已成为许多人和组织首选的开源关系数据库。


## 二、使用PostgreSQL有哪些好处？
- 1.PostgreSQL完全免费，与PostgreSQL配合的开源软件很多；
- 2.内核代码质量高，异常稳定；
- 3.丰富的扩展接口，用户可以用插件方式引入很多特性满足业务的需求；
- 4.兼具OLTP、OLAP场景，尤其新出的9.6版，OLAP场景能力大大提高；
- 5.能在功能和性能上满足绝大多数场景。

## 三、PostgreSQL的主要应用场景有哪些？

### 1.企业数据库
如ERP、交易系统、财务系统涉及资金、客户等信息，数据不能丢失且业务逻辑复杂，选择 PostgreSQL 作为数据底层存储，一是可以帮助您在数据一致性前提下提供高可用性，二是可以用简单的编程实现复杂的业务逻辑。

### 2.含LBS的应用
大型游戏、O2O等应用需要支持世界地图、附近的商家，两个点的距离等能力，PostGIS增加了对地理对象的支持，允许您以 SQL 运行位置查询，而不需要复杂的编码，帮助您更轻松理顺逻辑，更便捷的实现 LBS，提高用户粘性。

### 3.数据仓库和大数据
PostgreSQL更多的数据类型和强大的计算能力，能够帮助您更简单搭建数据库仓库或大数据分析平台，为企业运营加分。


### 4.建站或App
PostgreSQL良好的性能和强大的功能，可以有效的提高网站性能，降低开发难度。

## 四、如何安装PostgreSQL?
翻看我的博客，我发现我在2019年的时候就接触过PostgreSQL，那个时候之所以接触是因为当时研究学习并应用青岛OJ相关。

当时我是基于Ubuntu16.04环境安装的PostgreSQL，如今PostgreSQL不断更新，具体以参考官方文档为主(核心命令):
```
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum install -y postgresql14-server
sudo /usr/pgsql-14/bin/postgresql-14-setup initdb
sudo systemctl enable postgresql-14
sudo systemctl start postgresql-14

```

## 五、YC-Framework中如何使用PostgreSQL?

### 1.引入对应的模块依赖
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-postgresql</artifactId>
</dependency>

```

### 2.引入对应的ORM框架(MyBatis、MyBatis-Plus、JPA、Hibernate均支持)
YC-Framework里面使用的是MyBatis-Plus，也就是说你添加对应的MyBatis-Plus依赖即可。

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
</dependency>

```

### 3.核心配置文件
```
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/test
    username: postgres
    password: 123456

mybatis-plus:
  mapper-locations: classpath:/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: false

```

### 4.新建对应的库表
使用pdAdmin新建即可。

### 5.编写测试
```
@RestController
public class TestController {

    @Autowired
    private TestMapper testMapper;


    @PostMapping("/add")
    public Integer add(@RequestBody TestEntity testEntity) {
        return testMapper.insert(testEntity);
    }

    @GetMapping("/list")
    public List<TestEntity> list() {
        return testMapper.selectList(null);
    }

    @PutMapping("/update")
    public Integer update(@RequestBody TestEntity testEntity) {
        return testMapper.updateById(testEntity);
    }

    @DeleteMapping("/del")
    public Integer del(@RequestParam Integer id) {
        return testMapper.deleteById(id);
    }
}

```

相关示例代码地址:
https://github.com/developers-youcong/yc-framework/tree/main/yc-example/yc-example-postgresql

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework

以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star，鼓励一下！！！