---
title: SpringBoot整合Redisson(单机版)
date: 2020-11-06 20:05:21
tags: "SpringBoot"
---


## 一、为什么选择使用Redisson
因为它非常适用于分布式锁，而我们的一项业务需要考虑分布式锁这个应用场景，于是我整合它做一个初步简单的例子(和整合redis一样)。
<!--more-->

### Redisson、Jedis、Lettuce优缺点对比

#### (1)Redisson

优点：
实现了分布式特性和可扩展的 Java 数据结构，适合分布式开发；
API线程安全；
基于Netty框架的事件驱动的通信，可异步调用。

缺点：
API更抽象，学习使用成本高。
 

#### (2)Jedis

**优点：**
提供了比较全面的Redis操作特性的API
API基本与Redis的指令一一对应，使用简单易理解。

**缺点:**
同步阻塞IO；
不支持异步；
线程不安全。
 

#### (3)Lettuce

**优点：**
线程安全;
基于Netty 框架的事件驱动的通信，可异步调用;
适用于分布式缓存。

**缺点：**
API更抽象，学习使用成本高。

其中Jedis是用的最普遍的(确实非常简单)，特别是很多单体应用或者伪分布式应用等。

## 二、SpringBoot整合Redisson

### 1.添加Maven依赖
```
	<!-- redisson-springboot -->
    <dependency>
        <groupId>org.redisson</groupId>
        <artifactId>redisson-spring-boot-starter</artifactId>
        <version>3.11.4</version>
    </dependency>

```
### 2.配置文件
```
spring:
  redis:
    host: 127.0.0.1
    port: 6379
    database: 0
    timeout: 5000

```


### 3.添加配置类
```
import org.redisson.Redisson;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.data.redis.RedisProperties;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
public class RedissonConfig {

    @Autowired
    private RedisProperties redisProperties;

    @Bean
    public RedissonClient redissonClient() {
        Config config = new Config();
        String redisUrl = String.format("redis://%s:%s", redisProperties.getHost() + "", redisProperties.getPort() + "");
        config.useSingleServer().setAddress(redisUrl).setPassword(redisProperties.getPassword());
        config.useSingleServer().setDatabase(3);
        return Redisson.create(config);
    }

}

```
### 4.代码测试(简单的存取)
```
import org.redisson.api.RedissonClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/redisson")
public class RedissonController {

    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @GetMapping("/save")
    public String save(){
        stringRedisTemplate.opsForValue().set("key","redisson");
        return "save ok";
    }

    @GetMapping("/get")
    public String get(){
        return stringRedisTemplate.opsForValue().get("key");
    }

}

```