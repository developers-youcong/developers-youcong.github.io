---
title: 从零开始学YC-Framework之MongoDB
date: 2022-06-19 21:39:46
tags: "YC-Framework"
---

特别是针对大数据相关的应用场景产品，关系型数据库存在较大的瓶颈。而这一瓶颈的主要体现便是高并发的读写。YC-Framework针对这样的场景，提供对应的MongoDB方案，对应的MongoDB方案，在业界中也比较常用，符合技术选型的原则。
<!--more-->

## 一、MongoDB是什么？
MongoDB是一个基于分布式文件存储的数据库。采用C++语言编写。 旨在为WEB应用提供可扩展的高性能数据存储解决方案。MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。


## 二、MongoDB能解决什么问题？
- 1.对数据库高并发读写的需求。
- 2.对海量数据的高效率存储和访问的需求。
- 3.对数据库的高可扩展性和高可用性的需求。

## 三、实际应用案例包含哪些？
- 1.社交领域。
- 2.网络游戏。
- 3.物联网设备。
- 4.视频直播。
- 5.物流。

## 四、MongoDB如何安装？

### 1.下载安装包
```
cd /home/tech
# 下载安装包
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-5.0.2.tgz
# 解压安装包
tar -zxvf mongodb-linux-x86_64-rhel70-5.0.2.tgz
# 改名字
mv mongodb-linux-x86_64-rhel70-5.0.2 mongodb
# 此时可以删除安装包
rm -rf mongodb-linux-x86_64-rhel70-5.0.2.tgz

```

### 2.配置环境变量
```
vim /etc/profile

## 文件中写入
export MONGO_HOME=/home/tech/mongodb
export PATH=$PATH:$MONGO_HOME/bin;
## 然后按下shift+俩次 z 键，保存输入信息
# 更新
source /etc/profile

```

### 3.建立日志、数据文件、配置文件夹
```
# 在 /home/tech/mongodb 路径下创建，如果不是需要切换进入
cd /home/tech/mongodb
# 创建三个文件夹
mkdir logs data conf
# 进入conf文件夹（写全路径防止大家走错位置）
cd /home/tech/mongodb/conf
# 写配置文件信息
vim mongodb.conf
## 内容如下
port=27017 #端口
bind_ip=0.0.0.0 #默认是127.0.0.1
dbpath=/home/tech/mongodb/data #数据库存放
logpath=/home/tech/mongodb/logs/mongodb.log #日志文件
fork=true #设置后台运行
#auth=true #开启

```

### 4.启动mongo
```
启动命令
./mongod --config /home/tech/mongodb/conf/mongodb.conf
# 此处.conf文件路径一定不能出错

```

### 5.连接
```
mongo
```

## 五、YC-Framework如何使用MongoDB?

### 1.引入依赖
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-core</artifactId>
</dependency>

```
其中依赖中的核心之一便是：
```
<!-- MongoDB -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>

```

### 2.建立文档对象
```
@Data
@Document("operate_log")
public class OperateLog {
    private static final long serialVersionUID = 1L;

    /**
     * 日志主键
     */
    @Id
    private String operId;

    /**
     * 功能名称
     */
    private String functionName;

    /**
     * 请求方法
     */
    private String method;

    /**
     * 请求方式
     */
    private String requestMethod;

    /**
     * 操作人员
     */
    private String operName;

    /**
     * 公司名称
     */
    private String companyName;

    /**
     * 请求url
     */
    private String operUrl;

    /**
     * 操作地址
     */
    private String operIp;

    /**
     * 请求参数
     */
    private String operParam;

    /**
     * 返回参数
     */
    private String jsonResult;

    /**
     * 操作状态（0正常 1异常）
     */
    private Integer status;

    /**
     * 错误消息
     */
    private String errorMsg;

    /**
     * 操作时间
     */
    private Date operTime;
}


```

### 3.编写相关的业务接口类与实现类
OperateLogService.java
```
public interface OperateLogService {
    /**
     * 操作日志添加
     *
     * @param operLog
     * @return
     */
    OperateLog add(OperateLog operLog);
}


```

OperateLogServiceImpl.java
```
@Service
public class OperateLogServiceImpl implements OperateLogService {
    @Autowired
    private MongoTemplate mongoTemplate;

    @Override
    public OperateLog add(OperateLog operLog) {
        return mongoTemplate.save(operLog);
    }
}


```

### 4.controller测试
```
@RestController
@Slf4j
@Api(tags = {"后台管理-操作日志管理"}, description = "后台管理-操作日志管理")
public class OperateLogController {

    @Autowired
    private OperateLogService operateLogService;

    @PostMapping("/operate_log/add")
    @ApiOperation("操作日志数据添加")
    public RespBody add(@RequestBody OperateLog operateLog) {
        return RespBody.success(operateLogService.add(operateLog));
    }
}

```

在YC-Framework中使用MongoDB用于存储海量的操作日志，后续这些操作日志将会用于数据分析。

以上相关的步骤，适用于Java微服务体系实践落地MongoDB。


源代码均已开源，开源不易，如果对你有帮助，不妨给个star！！！

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework
