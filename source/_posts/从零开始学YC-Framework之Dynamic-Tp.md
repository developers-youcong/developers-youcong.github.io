---
title: 从零开始学YC-Framework之Dynamic-Tp
date: 2022-09-05 20:18:43
tags: "YC-Framework"
---

## 一、Dynamic-TP可以解决怎样的痛点问题？
- 1.代码中创建了一个 ThreadPoolExecutor，但是不知道那几个核心参数设置多少比较合适。
- 2.凭经验设置参数值，上线后发现需要调整，改代码重新发布服务，非常麻烦。
- 3.线程池相对开发人员来说是个黑盒，运行情况不能及时感知到，直到出现问题。

<!--more-->

## 二、Dynamic-TP的功能特性有哪些？
- 1.**代码零侵入：**所有配置都放在配置中心，对业务代码零侵入。
- 2.**轻量简单：**基于 springboot 实现，引入 starter，接入只需简单4步就可完成，顺利3分钟搞定。
- 3.**高可扩展：**框架核心功能都提供 SPI 接口供用户自定义个性化实现（配置中心、配置文件解析、通知告警、监控数据采集、任务包装等等）。
- 4.**线上大规模应用：**参考美团线程池实践，美团内部已经有该理论成熟的应用经验。
- 5.**多平台通知报警：**提供多种报警维度（配置变更通知、活性报警、容量阈值报警、拒绝触发报警、任务执行或等待超时报警），已支持企业微信、钉钉、飞书报警，同时提供 SPI 接口可自定义扩展实现。
- 6.**监控：**定时采集线程池指标数据，支持通过 MicroMeter、JsonLog 日志输出、Endpoint 三种方式，可通过 SPI 接口自定义扩展实现。
- 7.**任务增强：**提供任务包装功能，实现TaskWrapper接口即可，如 MdcTaskWrapper、TtlTaskWrapper、SwTraceTaskWrapper，可以支持线程池上下文信息传递。
- 8.**兼容性：**JUC 普通线程池和 Spring 中的 ThreadPoolTaskExecutor 也可以被框架监控，@Bean 定义时加 @DynamicTp 注解即可。
- 9.**可靠性：**框架提供的线程池实现 Spring 生命周期方法，可以在 Spring 容器关闭前尽可能多的处理队列中的任务。
- 10.**多模式：**参考Tomcat线程池提供了 IO 密集型场景使用的 EagerDtpExecutor 线程池。
- 11.**支持多配置中心：**基于主流配置中心实现线程池参数动态调整，实时生效，已支持 Nacos、Apollo、Zookeeper、Consul、Etcd，同时也提供 SPI 接口可自定义扩展实现。
- 12.**中间件线程池管理：**集成管理常用第三方组件的线程池，已集成Tomcat、Jetty、Undertow、Dubbo、RocketMq、Hystrix等组件的线程池管理（调参、监控报警）。


## 三、Dynamic-TP框架功能大体可以分为以下几个模块？
- 1.配置变更监听模块。
- 2.服务内部线程池管理模块。
- 3.三方组件线程池管理模块。
- 4.监控模块。
- 5.通知告警模块。

## 四、Dynamic-TP的技术架构图是怎样的？
![技术架构图](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38e4bf71d2c84b7ba67d7059b5432a7e~tplv-k3u1fbpfcp-zoom-1.image)

## 五、代码中如何接入Dynamic-TP？
一共五步，**如下所示:**

- 1.引入依赖。
- 2.编写配置类。
- 3.启动类加 @EnableDynamicTp 注解。
- 4.配置文件配置对应的内容。
- 5.使用 @Resource 或 @Autowired 进行依赖注入，或通过 DtpRegistry.getDtpExecutor("name")获取。

YC-Framework中只需引入对应的依赖就可以使用Dynamic-TP:
```
<dependency>
    <groupId>com.yc.framework</groupId>
	<artifactId>yc-common-dynamic-tp</artifactId>
</dependency>

```

这里我以配置类的方式来写一个例子（实际中这块会结合Nacos、Apollo或Zookeeper之类的）：

核心配置类:
```
@Configuration
public class DtpConfig {

    /**
     * 通过{@link DynamicTp} 注解定义普通juc线程池，会享受到该框架监控功能，注解名称优先级高于方法名
     *
     * @return 线程池实例
     */
    @DynamicTp("commonExecutor")
    @Bean
    public ThreadPoolExecutor commonExecutor() {
        return (ThreadPoolExecutor) Executors.newFixedThreadPool(1);
    }

    /**
     * 通过{@link ThreadPoolCreator} 快速创建一些简单配置的动态线程池
     * tips: 建议直接在配置中心配置就行，不用@Bean声明
     *
     * @return 线程池实例
     */
    @Bean
    public DtpExecutor dtpExecutor1() {
        return ThreadPoolCreator.createDynamicFast("dtpExecutor1");
    }

    /**
     * 通过{@link ThreadPoolBuilder} 设置详细参数创建动态线程池（推荐方式），
     * ioIntensive，参考tomcat线程池设计，实现了处理io密集型任务的线程池，具体参数可以看代码注释
     *
     * tips: 建议直接在配置中心配置就行，不用@Bean声明
     * @return 线程池实例
     */
    @Bean
    public DtpExecutor ioIntensiveExecutor() {
        return ThreadPoolBuilder.newBuilder()
                .threadPoolName("ioIntensiveExecutor")
                .corePoolSize(20)
                .maximumPoolSize(50)
                .queueCapacity(2048)
                .ioIntensive(true)
                .buildDynamic();
    }

    /**
     * tips: 建议直接在配置中心配置就行，不用@Bean声明
     * @return 线程池实例
     */
    @Bean
    public ThreadPoolExecutor dtpExecutor2() {
        return ThreadPoolBuilder.newBuilder()
                .threadPoolName("dtpExecutor2")
                .corePoolSize(10)
                .maximumPoolSize(15)
                .keepAliveTime(50)
                .timeUnit(TimeUnit.MILLISECONDS)
                .workQueue(QueueTypeEnum.SYNCHRONOUS_QUEUE.getName(), null, false)
                .waitForTasksToCompleteOnShutdown(true)
                .awaitTerminationSeconds(5)
                .buildDynamic();
    }
}


```

编写Controller进行测试:
```
@RestController
public class TestController {

    @GetMapping("/test")
    public String test() {
        DtpExecutor dtpExecutor = DtpRegistry.getDtpExecutor("dtpExecutor1");
        dtpExecutor.execute(() -> System.out.println("test"));
        return "test";
    }
}

```

**相关示例参考:**
https://github.com/developers-youcong/yc-framework/tree/main/yc-example/yc-example-dynamic-tp

以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star！！！

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework

## 六、关于Dynamic-TP相关的资料有哪些？

**官方文档：**

https://dynamictp.cn/guide/introduction/background.html

**源代码:**

https://github.com/dromara/dynamic-tp

https://gitee.com/dromara/dynamic-tp


**相关文章:**
https://dynamictp.cn/guide/other/articles.html