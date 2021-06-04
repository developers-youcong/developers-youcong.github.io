---
title: SpringBoot之整合Drools
date: 2020-07-04 11:22:20
tags: "SpringBoot"
---

## 一、Drools是什么
Drools是一个易于访问企业策略、易于调整以及易于管理的开源业务规则引擎，符合业内标准，速度快、效率高。业务分析师或审核人员可以利用它轻松查看业务规则，从而检验是否已编码的规则执行所需的业务规则。

## 二、Drools有什么用
从我个人所待过的公司，其中做智能酒店这个项目时就用到规则引擎Drools，将它用于处理优惠劵规则。
<!--more-->
## 三、SpringBoot整合Drools初步实战

### 1.导入Maven依赖
```
<properties>
<drools.version>7.14.0.Final</drools.version>
</properties>

<!-- drools -->
<dependency>
    <groupId>org.drools</groupId>
    <artifactId>drools-compiler</artifactId>
    <version>${drools.version}</version>
</dependency>

```

### 2.编写配置类
```
package com.springcloud.blog.admin.config;

import org.kie.api.KieBase;
import org.kie.api.KieServices;
import org.kie.api.builder.*;
import org.kie.api.runtime.KieContainer;
import org.kie.api.runtime.KieSession;
import org.kie.internal.io.ResourceFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternResolver;

import java.io.IOException;


@Configuration
public class KiaSessionConfig {

    private static final String RULES_PATH = "rules/";

    @Bean
    public KieFileSystem kieFileSystem() throws IOException {
        KieFileSystem kieFileSystem = getKieServices().newKieFileSystem();
        for (Resource file : getRuleFiles()) {
            kieFileSystem.write(ResourceFactory.newClassPathResource(RULES_PATH + file.getFilename(), "UTF-8"));
        }
        return kieFileSystem;
    }

    private Resource[] getRuleFiles() throws IOException {

        ResourcePatternResolver resourcePatternResolver = new PathMatchingResourcePatternResolver();
        final Resource[] resources = resourcePatternResolver.getResources("classpath*:" + RULES_PATH + "**/*.*");
        return resources;

    }

    @Bean
    public KieContainer kieContainer() throws IOException {

        final KieRepository kieRepository = getKieServices().getRepository();
        kieRepository.addKieModule(new KieModule() {
            public ReleaseId getReleaseId() {
                return kieRepository.getDefaultReleaseId();
            }
        });

        KieBuilder kieBuilder = getKieServices().newKieBuilder(kieFileSystem());
        kieBuilder.buildAll();
        return getKieServices().newKieContainer(kieRepository.getDefaultReleaseId());

    }

    private KieServices getKieServices() {
        return KieServices.Factory.get();
    }

    @Bean
    public KieBase kieBase() throws IOException {
        return kieContainer().getKieBase();
    }

    @Bean
    public KieSession kieSession() throws IOException {
        return kieContainer().newKieSession();
    }
}


```

### 3.resources目录新建rules目录


### 4.新建实体
```
package com.springcloud.blog.admin.drools;

public class People {
    private int sex;

    private String name;

    private String drlType;

    public int getSex() {
        return sex;
    }

    public void setSex(int sex) {
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDrlType() {
        return drlType;
    }

    public void setDrlType(String drlType) {
        this.drlType = drlType;
    }
}


```

### 5.编写规则文件
```
package com.springcloud.blog.admin.drools
import com.springcloud.blog.admin.drools.People
dialect  "java"

rule "man"
    when
        $p : People(sex == 1 && drlType == "people")
    then
        System.out.println($p.getName() + "是男孩");
end

```

### 6.单元测试(只要正常输出，表示整合是Ok的，接下来就可以任意应用了)
```
package com.springcloud.blog.base.controller.test.task;

import com.springcloud.blog.admin.BlogAdminApplication;
import com.springcloud.blog.admin.drools.People;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.kie.api.KieBase;
import org.kie.api.runtime.KieSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = BlogAdminApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class DroolsJunitTest {

    @Autowired
    private KieSession session;

    @Test
    public void people() {

        People people = new People();
        people.setName("YC");
        people.setSex(1);
        people.setDrlType("people");
        session.insert(people);//插入
        session.fireAllRules();//执行规则
    }


}


```

### 7.输出结果
```
YC是男孩

```
