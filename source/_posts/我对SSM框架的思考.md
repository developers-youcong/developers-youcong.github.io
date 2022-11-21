---
title: 我对SSM框架的思考
date: 2021-11-07 12:22:09
tags: "技术思考与实践"
---

## 一、SSM框架包含哪些？
通常说的SSM框架是指Spring、SpringMVC、MyBatis这三个的组合体，这三个组合起来，便是Java业界常说的三层架构，即表现层、业务逻辑层、数据存取层等。
<!--more-->

## 二、为什么要使用SSM框架？
SSM框架的诞生是为了解决SSH框架的局限性(Spring、Struts/Struts2、Hibernate)。

关于SpringMVC与Struts2的不同点、MyBatis与Hibernate的不同点比较，我在这篇文章[SSM框架整合+Ajax异步验证](https://www.cnblogs.com/youcong/p/7745731.html)写过了，这里不再赘述。

## 三、职业生涯中哪些项目涉及到SSM框架？
我与SSM框架的结缘，一个是基于最早某外包公司的OA、ERP这样的项目，另外一个就是创业公司的智能酒店系统。

## 四、SSM框架整合的例子以及涉及的相关问题有哪些？
我曾在博客园写下如下文章，仅供读者朋友参考:
[SSM框架整合+Ajax异步验证](https://www.cnblogs.com/youcong/p/7745731.html)
[SSM框架之整合(Maven实例)](https://www.cnblogs.com/youcong/p/8138270.html)
[SSM框架之整合EhCache](https://www.cnblogs.com/youcong/p/9931314.html)
[SSM框架之多数据源配置](https://www.cnblogs.com/youcong/p/9806848.html)
[SSM框架之RestFul示例](https://www.cnblogs.com/youcong/p/9348314.html)
[网站性能优化小结和Spring整合Redis](https://www.cnblogs.com/youcong/p/8511735.html)
[SSM框架之批量增加示例(同步请求jsp视图解析)](https://www.cnblogs.com/youcong/p/9356776.html)
[记一次关于SSM框架的使用错误](https://www.cnblogs.com/youcong/p/9588438.html)


## 五、理解Spring
我写过关于Spring相关有不少文章，相信下面的文章能带给读者朋友不一样的启发，文章列表如下:
[Spring(一)之概括与架构](https://www.cnblogs.com/youcong/p/9457820.html)
[Spring(二)之入门示例](https://www.cnblogs.com/youcong/p/9459296.html)
[Spring(三)之Ioc、Bean、Scope讲解](https://www.cnblogs.com/youcong/p/9459398.html)
[Spring(四)之Bean生命周期、BeanPost处理](https://www.cnblogs.com/youcong/p/9459813.html)
[Spring(五)之Bean定义继承和依赖注入](https://www.cnblogs.com/youcong/p/9459946.html)
[Spring(六)之自动装配](https://www.cnblogs.com/youcong/p/9460059.html)
[Spring(七)之基于注解配置](https://www.cnblogs.com/youcong/p/9460116.html)
[Spring(八)之基于Java配置](https://www.cnblogs.com/youcong/p/9460307.html)
[Spring(九)之事件处理](https://www.cnblogs.com/youcong/p/9460704.html)
[Spring(十)之自定义事件](https://www.cnblogs.com/youcong/p/9460716.html)
[Spring(十一)之AOP](https://www.cnblogs.com/youcong/p/9460773.html)
[Spring(十二)之JDBC框架](https://www.cnblogs.com/youcong/p/9460819.html)
[Spring(十三)之SQL存储过程](https://www.cnblogs.com/youcong/p/9460861.html)
[Spring(十四)之编程性事务(续)](https://www.cnblogs.com/youcong/p/9460920.html)
[Spring(十五)之声明式事务](https://www.cnblogs.com/youcong/p/9461014.html)
[Spring(十六)之MVC框架](https://www.cnblogs.com/youcong/p/9461114.html)
[Spring(十七)之表单处理](https://www.cnblogs.com/youcong/p/9461448.html)
[Spring(十八)之页面重定向](https://www.cnblogs.com/youcong/p/9461510.html)
[Spring(十九)之异常处理](https://www.cnblogs.com/youcong/p/9462715.html)
[Spring(二十)之使用Log4j记录日志](https://www.cnblogs.com/youcong/p/9462734.html)

## 六、理解SpringMVC
我写过关于SpringMVC相关有不少文章，相信下面的文章能带给读者朋友不一样的启发，文章列表如下:
[SpringMVC+Swagger详细整合](https://www.cnblogs.com/youcong/p/8088121.html)
[前后端交互之封装Ajax+SpringMVC源码分析](https://www.cnblogs.com/youcong/p/9265216.html)
[SpringMVC常见问题](https://www.cnblogs.com/youcong/p/9158457.html)
[SpringMVC与Ajax交互常见问题](https://www.cnblogs.com/youcong/p/9152813.html)
[SpringMVC整合FreeMarker实例](https://www.cnblogs.com/youcong/p/9416809.html)
[SpringMVC导出Excel（maven）](https://www.cnblogs.com/youcong/p/8325954.html)
[SpringMVC之ajax+select下拉框交互常用方式](https://www.cnblogs.com/youcong/p/9168872.html)
[关于SpringMVC返回数据带斜杠字符串问题之解决方案](https://www.cnblogs.com/youcong/p/9347675.html)
[关于SpringMVC整合freemarker报错问题](https://www.cnblogs.com/youcong/p/9416657.html)
[SpringMVC关于请求参数乱码问题](https://www.cnblogs.com/youcong/p/9629741.html)


## 七、理解MyBatis
我写过关于MyBatis下面的文章能带给读者朋友不一样的启发，文章列表如下:
[MyBatis之反射技术+JDK动态代理+cglib代理](https://www.cnblogs.com/youcong/p/8723177.html)
[MyBatis+Hibernate+JDBC对比分析](https://www.cnblogs.com/youcong/p/8719778.html)
[MyBatis实战之初步](https://www.cnblogs.com/youcong/p/9975973.html)
[MyBatis实战之配置](https://www.cnblogs.com/youcong/p/9977974.html)
[MyBatis实战之映射器](https://www.cnblogs.com/youcong/p/9985720.html)
[MyBatis实战之动态SQL](https://www.cnblogs.com/youcong/p/10003921.html)
[MyBatis实战之解析与运行](https://www.cnblogs.com/youcong/p/10046806.html)
更多关于MyBatis的内容，可访问这个地址[我的博客-MyBatis文章集合](https://www.cnblogs.com/youcong/category/1144041.html)

如果你对MyBatis-Plus感兴趣，可访问如下地址:
[我的思否专栏-MyBatis-Plus](https://segmentfault.com/u/youcongtech/articles)

MyBatis-Plus实战系列文章:
[MP实战系列(一)之入门框架搭建和使用](https://www.cnblogs.com/youcong/p/9010653.html)
[MP实战系列(二)之集成swagger](https://www.cnblogs.com/youcong/p/9011302.html)
[MP实战系列(三)之实体类讲解](https://www.cnblogs.com/youcong/p/9021739.html)
[MP实战系列(四)之DAO讲解](https://www.cnblogs.com/youcong/p/9026572.html)
[MP实战系列(五)之封装方法讲解](https://www.cnblogs.com/youcong/p/9029623.html)
[MP实战系列(六)之代码生成器讲解](https://www.cnblogs.com/youcong/p/9043051.html)
[MP实战系列(七)之集成SpringBoot](https://www.cnblogs.com/youcong/p/9061186.html)
[MP实战系列(八)之SpringBoot+Swagger2](https://www.cnblogs.com/youcong/p/9196157.html)
[MP实战系列(九)之集成Shiro](https://www.cnblogs.com/youcong/p/9240750.html)
[MP实战系列(十)之SpringMVC集成SpringFox+Swagger2](https://www.cnblogs.com/youcong/p/9255718.html)
关于MyBatis-Plus我一共写了二十篇，更多内容可访问该地址查看[我的博客-MP实战系列文章](https://www.cnblogs.com/youcong/category/1213059.html)

MyBatis-Plus不断更新迭代，上面是基于2.x版本，难免会有点落后，如果你想知道MyBatis-Plus版本特性，可通过[MyBatis-Plus官网](https://baomidou.com/)了解。

## 八、总结
仔细想来，写了四年的博客，实战类的文章并缺乏，这些年经历了从0到1或从1到2这样的开发历练，使我产生了很多思考，有对于基于Spring、SpringMVC、MyBatis源码的思考，也有基于技术与业务的思考，也有基于架构层面的思考，还有基于Java底层代码的思考等，而这些思考不断的促进我的认知，而这些认知使得我不断走的更远，取得了一些小成就。
