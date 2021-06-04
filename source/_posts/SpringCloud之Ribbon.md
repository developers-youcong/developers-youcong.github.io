---
title: SpringCloud之Ribbon
date: 2020-06-04 20:30:21
tags: "SpringCloud"
---
SpringCloud通过Ribbon实现负载均衡。
<!--more-->
## 一、添加Maven依赖
```
 <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
 </dependency>
 
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
        
  <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
  </dependency>

```

## 二、application.yml配置
```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8765
spring:
  application:
    name: blog-ribbon-client

```

## 三、主类编写
```
package com.springcloud.blog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.hystrix.EnableHystrix;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient
@EnableHystrix
@EnableCircuitBreaker
public class BlogRibbonClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(BlogRibbonClientApplication.class, args);
    }

    @Bean
    @LoadBalanced
    RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

```

## 四、编写测试相关

1.编写测试业务接口类
```
package com.springcloud.blog.example;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class TestService {
    @Autowired
    RestTemplate restTemplate;

    public String getPhone(String phone) {
        return restTemplate.getForObject("http://BLOG-API/juhe/getPhoneInfo?phone=" + phone, String.class);
    }


}


```

注意:
BLOG-API是服务实例的名字，是必须要在Eureka Server注册的，否则是无效的且请求会是404。

关于restTemplate分别实现RESTFUL相关，如POST、GET、DELETE、PUT等。

通过调用restTemplate.postForEntity(url,respResult)、restTemplate.getForEntity(url,respResult)、restTemplate.delete(url,respResult)、restTemplate.put(url,respResult)等，就能立马实现。
也可以使用全能的restTemplate.getForObject(url,respResult)。

其中的url一定要是eureka服务注册中心中存在的实例，同时实例后面相关的请求接口URL一定是要存在的，否则没有意义。

2.编写测试路由类
```
package com.springcloud.blog.example;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping(value = "/test")
public class TestController {

    @Autowired
    TestService testService;

    @GetMapping(value = "/getPhone")
    public String getPhone(String phone) {

        return testService.getPhone(phone);
    }
}

```

## 五、测试
请求的是我个人博客系统第三方集成API相关接口，图中的例子是输入个人手机号可获取该手机号相关信息
效果图如下:
![](SpringCloud之Ribbon/01.png)