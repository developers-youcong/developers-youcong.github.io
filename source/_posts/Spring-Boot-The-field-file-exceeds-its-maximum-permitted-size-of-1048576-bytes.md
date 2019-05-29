---
title: 'Spring Boot:The field file exceeds its maximum permitted size of 1048576 bytes'
date: 2019-04-08 21:11:29
tags: "SpringBoot"
---

错误信息:The field file exceeds its maximum permitted size of 1048576 bytes
原因是因为SpringBoot内嵌tomcat默认所能上传的文件大小为1M,超出这个就会报错。
<!--more-->
解决办法:

## 1.修改application.yml配置文件
```
spring:
  http:
    multipart:
      enabled: true
      max-file-size: 30MB
      max-request-size: 30MB

```

## 2.编写配置类
```
package com.blog.springboot.config;

import javax.servlet.MultipartConfigElement;

import org.springframework.boot.web.servlet.MultipartConfigFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MulterFile {
	/**  
     * 文件上传配置  
     * @return  
     */  
    @Bean  
    public MultipartConfigElement multipartConfigElement() {  
        MultipartConfigFactory factory = new MultipartConfigFactory();  
        //文件最大  
        factory.setMaxFileSize("30960KB"); //KB,MB  
        /// 设置总上传数据总大小  
        factory.setMaxRequestSize("309600KB");  
        return factory.createMultipartConfig();  
    }
}


```

参考资料:
[Spring Boot:The field file exceeds its maximum permitted size of 1048576 bytes.](https://blog.csdn.net/u010429286/article/details/54381705)
[Spring Boot设置上传文件大小](https://www.cnblogs.com/jiangwz/p/9030943.html)