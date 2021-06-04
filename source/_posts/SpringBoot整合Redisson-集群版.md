---
title: SpringBoot整合Redisson(集群版)
date: 2020-11-06 21:40:47
tags: "SpringBoot"
---

之前写了一篇关于SpringBoot整合Redisson的单机版，这篇是集群版。

关于如何在Linux搭建Redis集群，可以参考这篇文章:
[redis集群搭建](https://developers-youcong.github.io/2020/09/22/redis%E9%9B%86%E7%BE%A4%E6%90%AD%E5%BB%BA/)
<!--more-->

## 一、导入Maven依赖
```
  <!-- redisson-springboot -->
        <dependency>
            <groupId>org.redisson</groupId>
            <artifactId>redisson-spring-boot-starter</artifactId>
            <version>3.11.4</version>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-web</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-webflux</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

```

## 二、核心配置文件
```
 redis:
    cluster:
      nodes: "192.168.52.1:7000,192.168.52.1:7001,192.168.52.1:7002,192.168.52.1:7003,192.168.52.1:7004,192.168.52.1:7005"
    password: 123456
    lettuce:
      pool:
        max-active: 1500
        max-wait: 5000
        max-idle: 500
        min-idle: 100
        shutdown-timeout: 1000
    timeout: 60000

```

## 三、核心代码配置

RedisConfigProperties.java
```
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import java.util.List;
@Component
@ConfigurationProperties(prefix = "spring.redis")
public class RedisConfigProperties {
    private String password;
    private cluster cluster;

    public static class cluster {
        private List<String> nodes;

        public List<String> getNodes() {
            return nodes;
        }

        public void setNodes(List<String> nodes) {
            this.nodes = nodes;
        }
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public RedisConfigProperties.cluster getCluster() {
        return cluster;
    }

    public void setCluster(RedisConfigProperties.cluster cluster) {
        this.cluster = cluster;
    }
}

```

RedissonConfig.java
```
import org.redisson.Redisson;
import org.redisson.config.ClusterServersConfig;
import org.redisson.config.Config;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import java.util.ArrayList;
import java.util.List;
@Configuration
public class RedissonConfig {
    @Autowired
    private RedisConfigProperties redisConfigProperties;

    //添加redisson的bean
    @Bean
    public Redisson redisson() {
        //redisson版本是3.5，集群的ip前面要加上“redis://”，不然会报错，3.2版本可不加
        List<String> clusterNodes = new ArrayList<>();
        for (int i = 0; i < redisConfigProperties.getCluster().getNodes().size(); i++) {
            clusterNodes.add("redis://" + redisConfigProperties.getCluster().getNodes().get(i));
        }
        Config config = new Config();
        ClusterServersConfig clusterServersConfig = config.useClusterServers()
                .addNodeAddress(clusterNodes.toArray(new String[clusterNodes.size()]));
        clusterServersConfig.setPassword(redisConfigProperties.getPassword());//设置密码
        return (Redisson) Redisson.create(config);
    }
}

```

## 四、启动项目，只要不报错就表示配置集群成功，同时启动过程中也会显示连接的各个redis