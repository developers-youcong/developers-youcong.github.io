---
title: SpringBoot之整合MongoDB
date: 2020-05-21 20:26:58
tags: "SpringBoot"
---
MongoDB官网安装:
https://www.mongodb.com/download-center/community

MongoDB客户端工具(Mongo Management Studio)安装:
http://mms.litixsoft.de/#software_pricing

<!--more-->
## 一、添加Maven依赖
```
        <!--mongodb-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-mongodb</artifactId>
        </dependency>

```

## 二、application.yml配置
```
spring:
  data:
    mongodb:
      host: 127.0.0.1
      port: 27017
      database: blog

```

## 三、代码中使用Mongo

### 1.Mongo适用场景
(1)可用于动态列;
(2)可用于配置方面(如一些系统配置);
(3)日志记录;
(4)用于博客开发中的评论或留言;
(5)物联网方面的门锁相关信息存储;
(6)探头;

上面六个场景是我之前开发使用过的。

当然了，Mongo还可以应用更多地方，关键在于应用的场景是否合适。

### 2.以我最近博客开发的一个联系我为例(这里我使用Mongo)


#### (1)建立数据模型(需要在Mongo对应的库，建立对应的集合)
```
package com.springcloud.blog.admin.mongo.entity;

import com.springcloud.blog.admin.common.base.BaseDTO;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;

/**
 * 联系我-数据模型
 */
@Document(collection = "contact_me")
public class ContactMe extends BaseDTO {

    @Field
    private String name;

    @Field
    private String email;

    @Field
    private String content;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}


```

#### (2)数据访问层
```
package com.springcloud.blog.admin.mongo.dao;

import com.springcloud.blog.admin.mongo.entity.ContactMe;
        import org.springframework.data.mongodb.repository.MongoRepository;

/**
 * 联系我-持久层
 */
public interface ContactMeRepository extends MongoRepository<ContactMe, String> {
}

```
#### (3)对外API
```
package com.springcloud.blog.admin.mongo.controller;

import com.alibaba.fastjson.JSONObject;
import com.springcloud.blog.admin.common.dict.ResponseDict;
import com.springcloud.blog.admin.mongo.dao.ContactMeRepository;
import com.springcloud.blog.admin.mongo.entity.ContactMe;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * 联系我(Mongo Example)
 */
@RestController
@RequestMapping("/contactMe")
public class ContactMeController {


    @Autowired
    private ContactMeRepository contactMeRepository;

    @PostMapping("/save")
    public JSONObject save(@RequestBody ContactMe contactMe) {
        JSONObject jsonObject = new JSONObject();
        contactMeRepository.save(contactMe);
        jsonObject.put(ResponseDict.RESPONSE_CODE_KEY, ResponseDict.RESPONSE_SUCCESS_CODE);
        jsonObject.put(ResponseDict.RESPONSE_MSG_KEY, ResponseDict.RESPONSE_SUCCESS_MSG);
        return jsonObject;
    }


    @PostMapping("/delete")
    public JSONObject delete(@RequestBody ContactMe contactMe) {
        JSONObject jsonObject = new JSONObject();
        contactMeRepository.delete(contactMe);
        jsonObject.put(ResponseDict.RESPONSE_CODE_KEY, ResponseDict.RESPONSE_SUCCESS_CODE);
        jsonObject.put(ResponseDict.RESPONSE_MSG_KEY, ResponseDict.RESPONSE_SUCCESS_MSG);
        return jsonObject;
    }


    @PostMapping("/update")
    public JSONObject update(@RequestBody ContactMe contactMe) {
        JSONObject jsonObject = new JSONObject();
        contactMeRepository.save(contactMe);
        jsonObject.put(ResponseDict.RESPONSE_CODE_KEY, ResponseDict.RESPONSE_SUCCESS_CODE);
        jsonObject.put(ResponseDict.RESPONSE_MSG_KEY, ResponseDict.RESPONSE_SUCCESS_MSG);
        return jsonObject;
    }

    @PostMapping("/getAll")
    public JSONObject getAll() {
        JSONObject jsonObject = new JSONObject();
        List<ContactMe> list = contactMeRepository.findAll();
        jsonObject.put(ResponseDict.RESPONSE_CODE_KEY, ResponseDict.RESPONSE_SUCCESS_CODE);
        jsonObject.put(ResponseDict.RESPONSE_MSG_KEY, ResponseDict.RESPONSE_SUCCESS_MSG);
        jsonObject.put(ResponseDict.RESPONSE_DATA_KEY, list);
        return jsonObject;
    }
}



```

简单的说就是一个非常简单的增删改查，可帮助入门。
同时一般情况下，还是需要业务逻辑层的，一方面为了复用考虑，另外一方面不同的业务办不同的事情。