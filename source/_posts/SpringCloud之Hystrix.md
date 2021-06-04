---
title: SpringCloud之Hystrix
date: 2020-06-05 21:59:38
tags: "SpringCloud"
---

在微服务架构中，微服务之间互相依赖较大，相互之间调用必不可免的会失败。但当下游服务A因为瞬时流量导致服务崩溃，其他依赖于A服务的B、C服务由于调用A服务超时耗费了大量的资源，长时间下去，B、C服务也会崩溃。Hystrix就是用来解决服务之间相互调用失败，避免产生蝴蝶效应的熔断器，以及提供降级选项。Hystrix通过隔离服务之间的访问点，阻止它们之间的级联故障以及提供默认选项来实现这一目标，以提高系统的整体健壮性。

**它主要解决什么问题?**
<!--more-->
用来避免由于服务之间依赖较重，出现个别服务宕机、停止服务导致大面积服务雪崩的情况。

## 一、Maven依赖
```
  <dependencies>
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

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

    </dependencies>

```

## 二、启动类配置
```
package com.springcloud.blog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.openfeign.EnableFeignClients;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient
@EnableCircuitBreaker
@EnableFeignClients
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


## 三、修改配置文件(application.yml)
```
eureka:
  client:
    fetchRegistry: true
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
ribbon:
  okhttp:
    enabled: true #
  NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 默认为；轮询，这里改为随机
  ConnectTimeout: 5000 # 连接超时时间(ms)
  ReadTimeout: 5000 # 通信超时时间(ms)
hystrix:
  enabled: true
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 6000 #
server:
  port: 8765
spring:
  application:
    name: blog-ribbon-client

```

## 四、测试类
```
package com.springcloud.blog.example;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@FeignClient(value="blog-api")
public interface FeignTestService {

    @GetMapping("/juhe/getIpInfo")
    String getIpInfo(@RequestParam String ip);


}


```


## 五、测试效果
也可以自定义fallback实现返回值，只不过我觉得默认的看起来效果比较好，就直接默认的。
![](SpringCloud之Hystrix/01.png)