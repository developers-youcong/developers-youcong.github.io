---
title: 从零开始学YC-Framework之InfluxDB
date: 2022-11-30 17:58:17
tags: "YC-Framework"
---

## 一、InfluxDB是什么？
<!--more-->
InfluxDB是一个由InfluxData开发的开源时序型数据库，专注于海量时序数据的高性能读、高性能写、高效存储与实时分析等，广泛应用于DevOps监控、IoT监控、实时分析等场景。

## 二、InfluxDB具有哪些特性？
● Time Series （时间序列）：你可以使用与时间有关的相关函数（如最大，最小，求和等）
● Metrics（度量）：你可以实时对大量数据进行计算
● Eevents（事件）：它支持任意的事件数据

## 三、InfluxDB具有哪些特点？
● 为时间序列数据专门编写的自定义高性能数据存储。 TSM引擎具有高性能的写入和数据压缩
● Golang编写，没有其它的依赖
● 提供简单、高性能的写入、查询 http api，Native HTTP API, 内置http支持，使用http读写
● 插件支持其它数据写入协议，例如 graphite、collectd、OpenTSDB
● 支持类sql查询语句
● tags可以索引序列化，提供快速有效的查询
● Retention policies自动处理过期数据
● Continuous queries自动聚合，提高查询效率
● schemaless(无结构)，可以是任意数量的列
● Scalable可拓展
● min, max, sum, count, mean,median 一系列函数，方便统计
● Built-in Explorer 自带管理工具

## 四、InfluxDB与MySQL比较？
|  概念  | MySQL |InfluxDB |
|--------|--------|--------
|  概念      |   MySQL     |InfluxDB|
|  数据库（相同）      |   database     |database|
|  表（不同）      |   table     |measurement（测量; 度量）|
|  列（不同）      |   column     |tag(带索引的，非必须)、field(不带索引)、timestemp(唯一主键)| 


## 五、关于InfluxDB的相关资料有哪些？
InfluxDB官网:
https://www.influxdata.com/

InfluxDB文档:
https://docs.influxdata.com/

InfluxDB Github源代码:
https://github.com/influxdata/influxdb

## 六、如何快速安装InfluxDB?
```
wget https://dl.influxdata.com/influxdb/releases/influxdb2-2.3.0-linux-amd64.tar.gz

tar xvzf influxdb2-2.3.0-linux-amd64.tar.gz

sudo cp influxdb2-2.3.0-linux-amd64/influxd /usr/local/bin/

influxd

```

## 七、YC-Framework如何使用InfluxDB?

### 1.引入依赖
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-influxdb</artifactId>
</dependency>

```

### 2.配置文件
```
spring:
  influx:
    url: http://127.0.0.1:8086
    user: influxdb
    password: 123456ab
    database: yc-test

```

### 3.核心工具类
```
@Component
@Configuration
public class InfluxDBConfig {

    @Value("${spring.influx.user}")
    private String userName;

    @Value("${spring.influx.password}")
    private String password;

    @Value("${spring.influx.url}")
    private String url;

    //数据库
    @Value("${spring.influx.database}")
    private String database;

    //保留策略
    private String retentionPolicy;

    private InfluxDB influxDB;

    public InfluxDBConfig() {
    }

    public InfluxDBConfig(String userName, String password, String url, String database) {
        this.userName = userName;
        this.password = password;
        this.url = url;
        this.database = database;
        // autogen默认的数据保存策略
        this.retentionPolicy = retentionPolicy == null || "".equals(retentionPolicy) ? "autogen" : retentionPolicy;
        this.influxDB = influxDbBuild();
    }

    /**
     * 设置数据保存策略 defalut 策略名 /database 数据库名/ 30d 数据保存时限30天/ 1 副本个数为1/ 结尾DEFAULT
     * 表示 设为默认的策略
     */
    public void createRetentionPolicy() {
        String command = String.format("CREATE RETENTION POLICY \"%s\" ON \"%s\" DURATION %s REPLICATION %s DEFAULT",
                "defalut", database, "30d", 1);
        this.query(command);
    }

    /**
     * 连接时序数据库；获得InfluxDB
     **/
    private InfluxDB influxDbBuild() {
        if (influxDB == null) {
            influxDB = InfluxDBFactory.connect(url, userName, password);
            influxDB.setDatabase(database);
        }
        return influxDB;
    }

    /**
     * 插入
     *
     * @param measurement 表
     * @param tags        标签
     * @param fields      字段
     */
    public void insert(String measurement, Map<String, String> tags, Map<String, Object> fields) {
        influxDbBuild();
        Point.Builder builder = Point.measurement(measurement);
        builder.time(System.currentTimeMillis(), TimeUnit.MILLISECONDS);
        builder.tag(tags);
        builder.fields(fields);
        influxDB.write(database, "", builder.build());
    }

    /**
     * @param measurement
     * @param time
     * @param tags
     * @param fields
     * @return void
     * @desc 插入, 带时间time
     * @date 2021/3/27
     */
    public void insert(String measurement, long time, Map<String, String> tags, Map<String, Object> fields) {
        influxDbBuild();
        Point.Builder builder = Point.measurement(measurement);
        builder.time(time, TimeUnit.MILLISECONDS);
        builder.tag(tags);
        builder.fields(fields);
        influxDB.write(database, "", builder.build());
    }

    /**
     * @param measurement
     * @param time
     * @param tags
     * @param fields
     * @return void
     * @desc influxDB开启UDP功能, 默认端口:8089,默认数据库:udp,没提供代码传数据库功能接口
     * @date 2021/3/13
     */
    public void insertUDP(String measurement, long time, Map<String, String> tags, Map<String, Object> fields) {
        influxDbBuild();
        Point.Builder builder = Point.measurement(measurement);
        builder.time(time, TimeUnit.MILLISECONDS);
        builder.tag(tags);
        builder.fields(fields);
        int udpPort = 8089;
        influxDB.write(udpPort, builder.build());
    }

    /**
     * 查询
     *
     * @param command 查询语句
     * @return
     */
    public QueryResult query(String command) {
        influxDbBuild();
        return influxDB.query(new Query(command, database));
    }

    /**
     * @param queryResult
     * @desc 查询结果处理
     * @date 2021/5/12
     */
    public List<Map<String, Object>> queryResultProcess(QueryResult queryResult) {
        List<Map<String, Object>> mapList = new ArrayList<>();
        List<QueryResult.Result> resultList = queryResult.getResults();
        //把查询出的结果集转换成对应的实体对象，聚合成list
        for (QueryResult.Result query : resultList) {
            List<QueryResult.Series> seriesList = query.getSeries();
            if (seriesList != null && seriesList.size() != 0) {
                for (QueryResult.Series series : seriesList) {
                    List<String> columns = series.getColumns();
                    String[] keys = columns.toArray(new String[columns.size()]);
                    List<List<Object>> values = series.getValues();
                    if (values != null && values.size() != 0) {
                        for (List<Object> value : values) {
                            Map<String, Object> map = new HashMap(keys.length);
                            for (int i = 0; i < keys.length; i++) {
                                map.put(keys[i], value.get(i));
                            }
                            mapList.add(map);
                        }
                    }
                }
            }
        }
        return mapList;
    }

    /**
     * @desc InfluxDB 查询 count总条数
     * @date 2021/4/8
     */
    public long countResultProcess(QueryResult queryResult) {
        long count = 0;
        List<Map<String, Object>> list = queryResultProcess(queryResult);
        if (list != null && list.size() != 0) {
            Map<String, Object> map = list.get(0);
            double num = (Double) map.get("count");
            count = new Double(num).longValue();
        }
        return count;
    }

    /**
     * 查询
     *
     * @param dbName 创建数据库
     * @return
     */
    public void createDB(String dbName) {
        influxDbBuild();
        influxDB.createDatabase(dbName);
    }

    /**
     * 批量写入测点
     *
     * @param batchPoints
     */
    public void batchInsert(BatchPoints batchPoints) {
        influxDbBuild();
        influxDB.write(batchPoints);
    }

    /**
     * 批量写入数据
     *
     * @param database        数据库
     * @param retentionPolicy 保存策略
     * @param consistency     一致性
     * @param records         要保存的数据（调用BatchPoints.lineProtocol()可得到一条record）
     */
    public void batchInsert(final String database, final String retentionPolicy,
                            final InfluxDB.ConsistencyLevel consistency, final List<String> records) {
        influxDbBuild();
        influxDB.write(database, retentionPolicy, consistency, records);
    }

    /**
     * @param consistency
     * @param records
     * @desc 批量写入数据
     * @date 2021/3/19
     */
    public void batchInsert(final InfluxDB.ConsistencyLevel consistency, final List<String> records) {
        influxDbBuild();
        influxDB.write(database, "", consistency, records);
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getDatabase() {
        return database;
    }

    public void setDatabase(String database) {
        this.database = database;
    }

    public String getRetentionPolicy() {
        return retentionPolicy;
    }

    public void setRetentionPolicy(String retentionPolicy) {
        this.retentionPolicy = retentionPolicy;
    }

    public InfluxDB getInfluxDB() {
        return influxDB;
    }

    public void setInfluxDB(InfluxDB influxDB) {
        this.influxDB = influxDB;
    }
}


```


相关示例代码地址:
https://github.com/developers-youcong/yc-framework/tree/main/yc-example/yc-example-influxdb

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework

以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star，鼓励一下！！！