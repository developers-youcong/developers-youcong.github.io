---
title: 从零开始学YC-Framework之微服务通信
date: 2022-09-18 13:54:16
tags: "YC-Framework"
---

## 一、什么是微服务通信？
A服务调用B服务，B服务调C服务，C服务调D服务。换言之，这就是微服务之间的通信(也可以叫微服务之间的调用)。
<!--more-->

## 二、微服务的通信方式有哪些？
- 1.HTTP。
- 2.RPC。
- 3.Message。


## 三、实现这些通信方式的框架有哪些？
### 1.RPC
- (1)Dubbo；
- (2)Grpc；
- (3)Apache Thrift；
- (4)Hessian；
- (5)RMI。

### 2.HTTP
- (1)SpringCloud Open Feign；
- (2)SpringMVC；
- (3)Okhttp；
- (4)SpringBoot。

### 3.Message
- (1)JMS；
- (3)ActiveMQ；
- (4)RabbitMQ；
- (5)Kafka。

上述涉及的RPC、HTTP、Message相关的框架以及中间件等，YC-Framework大部分均已支持(以现主流为主)。

## 四、微服务的交互模式有哪些？微服务的分解和组合模式有哪些？微服务的容错模式有哪些？

### 1.微服务的交互模式
- 1.读者容错模式。
- 2.消费者驱动契约模式。
- 3.去数据共享模式。

### 2.微服务的分解和组合模式
- 1.服务代理模式。
- 2.服务聚合模式。
- 3.服务串联模式。
- 4.服务分支模式。
- 5.服务异步消息模式。
- 6.服务共享数据模式。

### 3.微服务的容错模式
- 1.舱壁隔离模式。
- 2.熔断模式。
- 3.限流模式。
- 4.失效转移模式。

上述三个部分内容的详细解答均可在我写的这篇文章（得到答案）：
[我在M2公司做架构之架构2.0](https://youcongtech.com/2021/10/16/%E6%88%91%E5%9C%A8M2%E5%85%AC%E5%8F%B8%E5%81%9A%E6%9E%B6%E6%9E%84%E4%B9%8B%E6%9E%B6%E6%9E%842-0/)

## 五、YC-Framework主要采用的微服务通信是基于什么？
YC-Framework使用Nacos作为服务注册与发现，通过Open Feign实现微服务之间的调用。

### 1.具体该如何使用呢？

#### (1)引入依赖
```

<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-nacos</artifactId>
</dependency>

<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-openfeign</artifactId>
</dependency>

```

#### (2)生产者服务模块启动类添加注解
```
@EnableFeignClients(basePackages ="com.ycframework.xxxxxx")

```

#### (3)在yc-api模块下新建对应的API类(这里以新建博客园API为例)
```
@FeignClient(contextId = "cnBlogsApi",name = ApplicationConst.PLUGINS)
public interface CnBlogsApi {
    @PostMapping("/cnblogs/getToken")
    RespBody getToken();

    @PostMapping("/cnblogs/getPersonalBlogInfo")
    RespBody getPersonalBlogInfo(@RequestParam("username") String username);

    @PostMapping("/cnblogs/getPersonalBlogPostList")
    RespBody getPersonalBlogPostList(@RequestParam("userName") String userName, @RequestParam("pageIndex") Integer pageIndex);

    @PostMapping("/cnblogs/getEssenceAreaPostList")
    RespBody getEssenceAreaPostList(@RequestParam("pageIndex") String pageIndex, @RequestParam("pageSize") String pageSize);

    @PostMapping("/cnblogs/getSiteHomePostList")
    RespBody getSiteHomePostList(@RequestParam("pageIndex") String pageIndex, @RequestParam("pageSize") String pageSize);
}

```

#### (4)消费者服务模块引入yc-api依赖即可
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-api</artifactId>
</dependency>

```
#### (5)调用
```
@Autowired
private CnBlogsApi cnBlogsApi;

```


### 2.案例有哪些？
在YC-Framework中，微服务通信的案例有如下:

- 1.插件服务(涉及博客园API、机器人聊天、和风天气API等)。
- 2.数据爬虫服务(通过调用插件服务API，实现数据存储入库)。
- 3.统一授权认证(认证服务、用户管理服务之间的API调用。
- 4.用户操作日志存储(用户操作系统做了什么的行为均存储到MongoDB)。

以上案例代码均可在YC-Framework中找到！！！

开源不易，如果对你有帮助，不妨给个star(Github与Gitee)，鼓励一下！！！

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework