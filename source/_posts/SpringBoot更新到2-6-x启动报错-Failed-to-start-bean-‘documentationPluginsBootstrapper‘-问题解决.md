---
title: >-
  SpringBoot更新到2.6.x启动报错 Failed to start bean ‘documentationPluginsBootstrapper‘
  问题解决
date: 2022-05-17 18:48:06
tags: "SpringBoot"
---

**错误背景：**
```
SpringBoot升级2.6.x版本并与Knife4j最新版本进行整合。

```
<!--more-->

**核心错误信息如下：**
```
org.springframework.context.ApplicationContextException: Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException


```

**解决办法（两种）：**

我个人采用第二种办法解决了这个报错问题！！！

第一种增加配置类，如下所示：
```
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport;

@Configuration
public class WebMvcConfigurer extends WebMvcConfigurationSupport {

    /**
     * 发现如果继承了WebMvcConfigurationSupport，则在yml中配置的相关内容会失效。 需要重新指定静态资源
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**").addResourceLocations(
                "classpath:/static/");
        registry.addResourceHandler("swagger-ui.html", "doc.html").addResourceLocations(
                "classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations(
                "classpath:/META-INF/resources/webjars/");
        super.addResourceHandlers(registry);
    }

}


```

第二种配置文件增加对应的配置即可，如下所示：
```
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher

```
