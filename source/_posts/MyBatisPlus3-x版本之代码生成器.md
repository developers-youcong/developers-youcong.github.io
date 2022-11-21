---
title: MyBatisPlus3.x版本之代码生成器
date: 2022-03-16 19:12:02
tags: "MyBatis-Plus"
---

代码生成器就是为了提高开发者的效率!!!
<!--more-->

## 一、导入Maven依赖
```
<properties>
    <lombok.version>1.18.10</lombok.version>
    <hutool-all.version>5.7.9</hutool-all.version>
    <mp.version>3.2.0</mp.version>
    <mysql.version>8.0.19</mysql.version>
    <velocity.version>1.7</velocity.version>
</properties>

<dependencies>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>${mp.version}</version>
</dependency>

<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>${mp.version}</version>
</dependency>

<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>${mysql.version}</version>
</dependency>


<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>${lombok.version}</version>
</dependency>

<!--模版 -->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity</artifactId>
    <version>${velocity.version}</version>
</dependency>

<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>${hutool-all.version}</version>
</dependency>

</dependencies>

```

## 二、新建配置文件
```
# 数据库配置
jdbc_driver=com.mysql.cj.jdbc.Driver
jdbc_url=jdbc:mysql://127.0.0.1:3306/yc-framework?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8
jdbc_user=yc-framework
jdbc_pwd=xxxxxx

```

## 三、核心源代码
```
package com.yc.code.generator;

import cn.hutool.setting.dialect.Props;
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

/**
 * @description:
 * @author: youcong
 */
public class YcCodeGenerator {
    public static void main(String[] args) {
        // 创建代码生成器
        AutoGenerator mpg = new AutoGenerator();
        //全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("youcong");
        gc.setOpen(false); //生成后是否打开资源管理器
        gc.setFileOverride(false); //重新生成时文件是否覆盖
        gc.setServiceName("%sService"); //去掉Service接口的首字母I
        gc.setIdType(IdType.ID_WORKER_STR); //主键策略
        gc.setDateType(DateType.ONLY_DATE);//定义生成的实体类中日期类型
        gc.setSwagger2(false);//开启Swagger2模式
        gc.setEntityName("%sEntity");
        gc.setMapperName("%sMapper");
        mpg.setGlobalConfig(gc);
        //读取配置文件
        Props props = new Props("code.properties");
        //数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setDriverName(props.get("jdbc_driver").toString());
        dsc.setUrl(props.get("jdbc_url").toString());
        dsc.setUsername(props.get("jdbc_user").toString());
        dsc.setPassword(props.get("jdbc_pwd").toString());
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);
        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName(null); //模块名
        pc.setParent("com.yc.xx");
        pc.setController("controller");
        pc.setEntity("entity");
        pc.setService("service");
        pc.setMapper("dao");
        pc.setXml("dao");
        mpg.setPackageInfo(pc);
        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setInclude("yc_user");//对那一张表生成代码
        strategy.setNaming(NamingStrategy.underline_to_camel);//数据库表映射到实体的命名策略
        strategy.setTablePrefix("yc" + "_"); //生成实体时去掉表前缀
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);//数据库表字段映射到实体的命名策略
        strategy.setEntityLombokModel(true); // lombok 模型 @Accessors(chain = true) setter链式操作
        strategy.setRestControllerStyle(true); //restful api风格控制器
        strategy.setControllerMappingHyphenStyle(true); //url中驼峰转连字符
        mpg.setStrategy(strategy);
        // 执行
        mpg.execute();
    }
}


```


源代码地址:
https://github.com/developers-youcong/yc-framework/tree/main/yc-modules/yc-code-generator