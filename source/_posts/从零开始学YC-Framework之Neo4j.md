---
title: 从零开始学YC-Framework之Neo4j
date: 2022-10-16 14:49:14
tags: "YC-Framework"
---

## 一、什么是Neo4j?
Neo4j是一个高性能的，NOSQL数据库，它将结构化数据存储在网络上而不是表中。它是一个嵌入式的、基于磁盘的、具备完全的事物特性的java持久化引擎。Neo4j也可以看作是一个高性能的图引擎，该引擎具有成熟数据库所有特性。
<!--more-->

## 二、Neo4j的使用场景有哪些？
- 1.社交媒体和社交网络图。
- 2.知识图。
- 3.反欺诈多维关联分析。
- 4.企业关系图谱。
- 5.征信系统。
- 6.客户分析。
- 7.产品推荐。
- 8.风险管理。

## 三、Neo4j有哪些特点？
- 1.SQL就像简单的查询语言Neo4j CQL。
- 2.它遵循属性图数据模型。
- 3.它通过使用Apache Lucence支持索引。
- 4.它支持UNIQUE约束。
- 5.它包含一个用于执行CQL命令的UI：Neo4j数据浏览器。
- 6.它支持完整的ACID（原子性，一致性，隔离性和持久性）规则。
- 7.它采用原生图形库与本地GPE（图形处理引擎）。
- 8.它支持查询的数据导出到JSON和XLS格式。
- 9.它提供了REST API，可以被任何编程语言（如Java，Spring，Scala等）访问。
- 10.它提供了可以通过任何UI MVC框架（如Node JS）访问的Java脚本。
- 11.它支持两种Java API：Cypher API和Native Java API来开发Java应用程序。


## 四、Neo4j的优缺点有哪些？

### 1.优点
- (1)表示连接的数据很容易。
- (2)检索/遍历/浏览更多已连接数据非常简单快捷。
- (3)它很容易地代表半结构化数据。
- (4)Neo4j CQL查询语言命令是人性化的可读格式，非常容易学习。
- (5)它使用简单而强大的数据模型。
- (6)它不需要复杂的连接来检索连接/相关数据，因为在没有连接或索引的情况下很容易检索它的相邻节点或关系细节。

### 2.缺点
- (1)AS的Neo4j 2.1.3最新版本，它支持节点数，关系和属性的限制。
- (2)它不支持Sharding。

## 五、YC-Framework如何使用Neo4j？

### 1.导入Maven依赖
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-neo4j</artifactId>
</dependency>

```

### 2.核心配置文件
```
spring:
  neo4j:
    uri: bolt://127.0.0.1:7687
    authentication:
      username: neo4j
      password: neo4j


```

### 3.实体对象
```
@NodeEntity(label = "dept")
@AllArgsConstructor
@NoArgsConstructor
@Data
@Builder
public class Dept {

    @Id
    @GeneratedValue
    private Long id;

    @Property(name = "deptName")
    private String deptName;


}

```


### 4.测试Controller
```
@RestController
public class TestController {
    @Resource
    private DeptRepository deptRepository;
    @Resource
    private RelationShipRepository relationShipRepository;

    @GetMapping("create")
    public void create() {
        Dept root = new Dept(1L, "软件部");
        Dept dept1 = new Dept(2L, "架构组");
        Dept dept2 = new Dept(3L, "开发组");
        Dept dept3 = new Dept(4L, "实施组");

        List<Dept> deptList = new ArrayList<>(Arrays.asList(root, dept1, dept2, dept3));
        deptRepository.saveAll(deptList);

        RelationShip relationShip1 = new RelationShip(1L, root, dept1);
        RelationShip relationShip2 = new RelationShip(2L, root, dept2);
        RelationShip relationShip3 = new RelationShip(3L, root, dept3);

        List<RelationShip> relationShipList = new ArrayList<>(Arrays.asList(relationShip1, relationShip2, relationShip3));
        relationShipRepository.saveAll(relationShipList);
    }


    @GetMapping("/list")
    public List<Dept> list() {
        return (List<Dept>) deptRepository.findAll();
    }


    @GetMapping("deleteRelationShip")
    public void deleteRelationShip(Long id) {
        relationShipRepository.deleteById(id);
    }

    @GetMapping("deleteDept")
    public void deleteDept(Long id) {
        deptRepository.deleteById(id);
    }

    @GetMapping("deleteAll")
    public void deleteAll() {
        deptRepository.deleteAll();
        relationShipRepository.deleteAll();
    }

}


```

相关示例代码地址:
https://github.com/developers-youcong/yc-framework/tree/main/yc-example/yc-example-neo4j

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework

以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star，鼓励一下！！！