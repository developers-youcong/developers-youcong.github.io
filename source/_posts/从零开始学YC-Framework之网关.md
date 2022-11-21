---
title: 从零开始学YC-Framework之网关
date: 2022-09-18 15:30:25
tags: "YC-Framework"
---

## 一、网关是什么？
所有微服务的入口。
<!--more-->

## 二、网关的目的是什么？
- 1.统一入口(所有微服务的入口点)；
- 2.鉴权校验(统一请求接口鉴权)；
- 3.动态路由(统一请求接口分发)；
- 4.降低耦合(服务独立发展，网关映射即可)。

## 三、网关的优缺点有哪些？

### 1.优点
- (1)避免将内部信息泄露给外部；
- (2)为微服务添加额外的安全层；
- (3)支持混合通信协议；
- (4)降低构建微服务的复杂性；
- (5)微服务模拟与虚拟化。

### 2.缺点
- (1)架构上需要额外考虑更多的编排与管理；
- (2)路由逻辑配置要进行统一的管理；
- (3)可能引发单点故障。


## 四、网关现有的解决方案有哪些？
- (1)SpringCloud Gateway；
- (2)SpringCloud Zuul；
- (3)Kong；
- (4)Apache Shenyu；
- (5)Apache Apisix；
- (6)Nginx；
- (7)Openrestry；
- (8)Tyk；
- (9)Gravitee；
- (10)Express-Gateway；
- (11)Gloo；
- (12)KrakenD；
- (13)WSO2；
- (14)BFE。

## 五、YC-Framework中使用什么技术作为网关？
YC-Framework使用SpringCloud Gateway作为网关技术。

### 1.我个人在应用SpringCloud Gateway作为网关遇到了哪些问题？
- (1)网关与微服务没有映射成功导致访问404；
- (2)网关因为磁盘空间不足陷入”假死”状态导致所有服务不可用；
- (3)网关某一个跟登录相关的接口报错导致系统无法使用；
- (4)网关所涉及的中间件被黑客攻击导致服务器崩溃，从而使得所有服务均不可用；
- (5)网关503错误；
- (6)网关504错误；
- (7)网关跨域问题(SpringCloud Gateway与Nginx相关配置)；
- (8)网关请求体超出长度限制问题。

而上面这些问题都被我的方法论一一清理掉，使用过程中有任何问题均可留言给我！！！

### 2.有读者问我不想使用SpringCloud Gateway作为网关是否可以？
我的回答是，当然可以，而且不用担心鉴权会受到影响。因为鉴权早已模块化。只要你对应的微服务引入了鉴权模块就不需要担心。无论采用何种技术作为网关，配置相关大同小异罢了。

### 3.网关在YC-Framework中的位置是什么？
位置很重要，内外连接的关键点。看YC-Framework的架构图就能知道:
![架构图](https://framework.youcongtech.com/_media/%E6%8A%80%E6%9C%AF%E6%9E%B6%E6%9E%84%E5%9B%BE-V1.0.jpg)

### 4.YC-Framework的网关核心代码有哪些？

#### (1)核心依赖
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>

```

#### (2)核心配置
```
spring:
  cloud:
    gateway:
      discovery:
        locator:
          lowerCaseServiceId: true
          enabled: true
      routes:
        ## auth
        - id: yc-auth
          uri: lb://yc-auth
          predicates:
            - Path=/cert/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1
        ## admin
        - id: yc-admin
          uri: lb://yc-admin
          predicates:
            - Path=/admin/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1
        ## cms
        - id: yc-cms
          uri: lb://yc-cms
          predicates:
            - Path=/cms/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1
        ## file
        - id: yc-file
          uri: lb://yc-file
          predicates:
            - Path=/file/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1
# 打开客户端的监控
management:
  endpoints:
    sensitive: false
    web:
      exposure:
        include: '*'



```

#### (3)统一异常处理
```
@Order(-1)
@Configuration
public class GatewayExceptionHandler implements ErrorWebExceptionHandler {
    private static final Logger log = LoggerFactory.getLogger(GatewayExceptionHandler.class);

    @Override
    public Mono<Void> handle(ServerWebExchange exchange, Throwable ex) {
        ServerHttpResponse response = exchange.getResponse();

        if (exchange.getResponse().isCommitted()) {
            return Mono.error(ex);
        }

        String msg;

        if (ex instanceof NotFoundException) {
            msg = "服务未找到";
        } else if (ex instanceof ResponseStatusException) {
            ResponseStatusException responseStatusException = (ResponseStatusException) ex;
            msg = responseStatusException.getMessage();
        } else {
            msg = "内部服务器错误";
        }

        log.error("[网关异常处理]请求路径:{},异常信息:{}", exchange.getRequest().getPath(), ex.getMessage());

        response.getHeaders().setContentType(MediaType.APPLICATION_JSON);
        response.setStatusCode(HttpStatus.OK);
        return response.writeWith(Mono.fromSupplier(() -> {
            DataBufferFactory bufferFactory = response.bufferFactory();
            return bufferFactory.wrap(JSON.toJSONBytes(RespBody.fail(msg)));
        }));
    }
}

```

其它相关代码就不一一列举了，有的与Knife4j有关，有的与Sentinel有关，具体大家可以看源码！！！

yc-gateway源代码地址:
https://github.com/developers-youcong/yc-framework/tree/main/yc-gateway

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework



以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star，鼓励一下！！！