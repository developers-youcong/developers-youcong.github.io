---
title: 我对SpringBoot的思考
date: 2021-11-06 19:57:17
tags: "技术思考与实践"
---

## 一、什么是SpringBoot?
在Spring框架这个大家族中，产生了很多衍生框架，比如Spring、SpringMvc框架等，Spring的核心内容在于控制反转(IOC)和依赖注入(DI),所谓控制反转并非是一种技术，而是一种思想，在操作方面是指在spring配置文件中创建bean，依赖注入即为由Spring容器为应用程序的某个对象提供资源，比如引用对象、常量数据等。
<!--more-->

SpringBoot是一个框架，一种全新的编程规范，他的产生简化了框架的使用，所谓简化是指简化了Spring众多框架中所需的大量且繁琐的配置文件，所以 SpringBoot是一个服务于框架的框架，服务范围是简化配置文件。


## 二、SpringBoot是怎么诞生的？
**任何技术的诞生都是为了解决具体的问题**。例如SpringBoot的诞生是为了解决Java开发中的繁多的配置、低下的开发效率，复杂的部署流程、第三方技术集成难度大等问题。

## 三、职业生涯中我在哪些项目中用到SpringBoot?
- 智能门锁系统；
- 资源管理系统；
- 共享洗浴间项目；
- 智浴平台；
- 编程教育项目；
- 魔改系统；
- 教育Saas系统；
- 某大数据分析Saas系统。

一共8个商业级系统，关于前六个，也就是智能门锁到魔改系统这块，是我在创业公司的经历，下面这两篇文章，感兴趣的朋友可以阅读:
[创业公司这两年-简要概括-免费文章](https://mp.weixin.qq.com/s?__biz=MzUxODk0ODQ3Ng==&mid=2247484272&idx=1&sn=f9e87cb8e838ec36164435d7d75d37fd&chksm=f9805063cef7d9753e098398ebb61508ab9271c5c123762493247381338238922e5c1e95da75&token=681737576&lang=zh_CN#rd)

[创业公司这两年-详细概括-付费文章](https://mp.weixin.qq.com/s?__biz=MzUxODk0ODQ3Ng==&mid=2247485439&idx=1&sn=201a791086509ae93a29a669e8d402be&chksm=f98054eccef7ddfac7c6b7d30d40b33a1044cc702850cadf1ee2b0a9b3caca551f56779c719b&token=2104650779&lang=zh_CN#rd)

## 四、我曾写过关于SpringBoot相关的文章有哪些？
下面这些文章都是我早年写的，感兴趣的朋友可以看看，但是技术这东西随着时间会不断发生变化的，有的开源技术随着时间不断更新迭代，有的开源技术随着时间慢慢的不再活跃直至死掉。

[SpringBoot+MyBatis整合实例](https://www.cnblogs.com/youcong/p/8644216.html)
[SpringBoot入门程序](https://www.cnblogs.com/youcong/p/8098861.html)
[SpringBoot实战(一)之构建RestFul风格](https://www.cnblogs.com/youcong/p/9385327.html)
[SpringBoot实战(二)之计划任务](https://www.cnblogs.com/youcong/p/9385472.html)
[SpringBoot实战(三)之使用RestFul Web服务](https://www.cnblogs.com/youcong/p/9385579.html)
[SpringBoot实战(四)之使用JDBC和Spring访问数据库](https://www.cnblogs.com/youcong/p/9385710.html)
[SpringBoot实战(五)之Thymeleaf](https://www.cnblogs.com/youcong/p/9385918.html)
[SpringBoot实战(六)之使用LDAP验证用户](https://www.cnblogs.com/youcong/p/9386186.html)
[SpringBoot实战(七)之与Redis进行消息传递](https://www.cnblogs.com/youcong/p/9386398.html)
[SpringBoot实战(八)之RabbitMQ](https://www.cnblogs.com/youcong/p/9387611.html)
[SpringBoot实战(九)之Validator](https://www.cnblogs.com/youcong/p/9387896.html)
[SpringBoot实战(十)之使用Spring Boot Actuator构建RESTful Web服务](https://www.cnblogs.com/youcong/p/9409550.html)
[SpringBoot实战(十一)之与JMS简单通信](https://www.cnblogs.com/youcong/p/9409694.html)
[SpringBoot实战(十二)之集成kisso](https://www.cnblogs.com/youcong/p/9794776.html)
[SpringBoot实战(十三)之缓存](https://www.cnblogs.com/youcong/p/9899053.html)
[SpringBoot实战(十四)之整合KafKa](https://www.cnblogs.com/youcong/p/10216573.html)
[SpringBoot之热部署](https://www.cnblogs.com/youcong/p/11523994.html)
[SpringBoot整合Xxl-Job](https://www.cnblogs.com/youcong/p/12935760.html)
[SpringBoot之整合Dubbo](https://www.cnblogs.com/youcong/p/12935789.html)
[SpringBoot之整合MongoDB](https://www.cnblogs.com/youcong/p/12935733.html)
[SpringBoot+MyBatis-Plus实现多数据源](https://www.cnblogs.com/youcong/p/12935716.html)
[SpringBoot整合Apache-CXF实践](https://www.cnblogs.com/youcong/p/13874940.html)
[SpringBoot应用之运行jar包时指定端口](https://www.cnblogs.com/youcong/p/13654088.html)
[SpringBoot+MyBatis+Redis(二级缓存)](https://www.cnblogs.com/youcong/p/13654080.html)
[SpringBoot单文件与多文件上传](https://www.cnblogs.com/youcong/p/14801485.html)
[SpringBoot之AOP使用](https://www.cnblogs.com/youcong/p/11488554.html)


最新的学习，还请参考官网上的:
[SpringBoot官网](https://spring.io/projects/spring-boot)
[SpringBoot Github](https://github.com/spring-projects/spring-boot)

## 五、SpringBoot常用的注解有哪些？
- @Configuration；
- @ComponentScan；
- @Conditional；
- @Import；
- @ImportResource；
- @Component；
- @SpringBootApplication；
- @EnableAutoConfiguration；
- @SpringBootConfiguration；
- @ConditionalOnBean；
- @ConditionalOnMissingBean；
- @ConditionalOnClass；
- @ConditionalOnMissingClass；
- @ConditionalOnWebApplication；
- @ConditionalOnNotWebApplication；
- @ConditionalOnProperty；
- @ConditionalOnExpression；
- @ConditionalOnJava；
- @ConditionalOnResource；
- @ConditionalOnJndi；
- @ConditionalOnCloudPlatform；
- @ConditionalOnSingleCandidate；
- @ConfigurationProperties；
- @EnableConfigurationProperties；
- @AutoConfigureAfter；
- @AutoConfigureBefore；
- @AutoConfigureOrder。

关于上面的注解是什么意思以及实际中如何使用，可以通过相关搜索引擎得到答案，这里不再赘述。

## 六、SpringBoot与微服务
传统的SSM框架，配置繁琐，开发效率低，打包部署流程繁琐，第三方库并不是很友好等，不能适应微服务的需要。微服务是应分布式而生的。关于这一点我在如下文章已经较为详细说明过了，可以阅读:
[从单体架构到分布式微服务架构的思考](https://youcongtech.com/2021/04/17/%E4%BB%8E%E5%8D%95%E4%BD%93%E6%9E%B6%E6%9E%84%E5%88%B0%E5%88%86%E5%B8%83%E5%BC%8F%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E7%9A%84%E6%80%9D%E8%80%83/)

我所写过关于微服务相关的思考文章如下:
[质疑微服务设计](https://youcongtech.com/2021/03/27/%E8%B4%A8%E7%96%91%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%AE%BE%E8%AE%A1/)
[如何写好对外的API微服务](https://youcongtech.com/2021/03/27/%E5%A6%82%E4%BD%95%E5%86%99%E5%A5%BD%E5%AF%B9%E5%A4%96%E7%9A%84API%E5%BE%AE%E6%9C%8D%E5%8A%A1/)
[深入理解SaaS之架构篇](https://youcongtech.com/2021/08/02/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3SaaS%E4%B9%8B%E6%9E%B6%E6%9E%84%E7%AF%87/)
[BASE理论之思考](https://youcongtech.com/2021/06/03/BASE%E7%90%86%E8%AE%BA%E4%B9%8B%E6%80%9D%E8%80%83/)
[CAP理论之思考](https://youcongtech.com/2021/06/03/CAP%E7%90%86%E8%AE%BA%E4%B9%8B%E6%80%9D%E8%80%83/)
[我在M2公司做架构系列文章](https://youcongtech.com/tags/%E6%8A%80%E6%9C%AF%E6%80%9D%E8%80%83%E4%B8%8E%E5%AE%9E%E8%B7%B5/)

## 七、关于SpringBoot工作原理
用一句话概括，核心注解+执行流程。
核心注解主要看@SpringBootApplication注解，执行流程是关于启动流程。后面我会有专门的文章围绕这两个方面进行详细讲解。

## 八、总结
在这个充满焦虑而又内卷的社会里，不能总学这学哪，受制于人，也得找个时间停一停(用砍柴师傅的一句话来说，叫做磨刀不误砍柴工)，不断的回顾过去、反思过去、总结过去，形成自己的一套方法论，这样的话，或许能让前路更清晰。