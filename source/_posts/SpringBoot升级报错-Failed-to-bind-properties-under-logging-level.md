---
title: 'SpringBoot升级报错:Failed to bind properties under ''logging.level'''
date: 2019-10-18 17:16:00
tags: "SpringBoot"
---

错误详细信息:
<!--more-->
```
org.springframework.boot.context.properties.bind.BindException: Failed to bind properties under 'logging.level' to java.util.Map<java.lang.String, java.lang.String>
	at org.springframework.boot.context.properties.bind.Binder.handleBindError(Binder.java:250) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.bind(Binder.java:226) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.bind(Binder.java:210) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.bind(Binder.java:166) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.logging.LoggingApplicationListener.setLogLevels(LoggingApplicationListener.java:307) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.logging.LoggingApplicationListener.initializeFinalLoggingLevels(LoggingApplicationListener.java:290) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.logging.LoggingApplicationListener.initialize(LoggingApplicationListener.java:238) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.logging.LoggingApplicationListener.onApplicationEnvironmentPreparedEvent(LoggingApplicationListener.java:200) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.logging.LoggingApplicationListener.onApplicationEvent(LoggingApplicationListener.java:173) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.context.event.SimpleApplicationEventMulticaster.doInvokeListener(SimpleApplicationEventMulticaster.java:172) ~[spring-context-5.0.11.RELEASE.jar:5.0.11.RELEASE]
	at org.springframework.context.event.SimpleApplicationEventMulticaster.invokeListener(SimpleApplicationEventMulticaster.java:165) ~[spring-context-5.0.11.RELEASE.jar:5.0.11.RELEASE]
	at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:139) ~[spring-context-5.0.11.RELEASE.jar:5.0.11.RELEASE]
	at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:127) ~[spring-context-5.0.11.RELEASE.jar:5.0.11.RELEASE]
	at org.springframework.boot.context.event.EventPublishingRunListener.environmentPrepared(EventPublishingRunListener.java:74) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.SpringApplicationRunListeners.environmentPrepared(SpringApplicationRunListeners.java:54) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.SpringApplication.prepareEnvironment(SpringApplication.java:338) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:297) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at com.acs.springboot.AcsAdminApplication.main(AcsAdminApplication.java:20) [classes/:na]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_152]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_152]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_152]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_152]
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:49) [spring-boot-devtools-2.0.7.RELEASE.jar:2.0.7.RELEASE]
Caused by: org.springframework.core.convert.ConverterNotFoundException: No converter found capable of converting from type [java.lang.String] to type [java.util.Map<java.lang.String, java.lang.String>]
	at org.springframework.core.convert.support.GenericConversionService.handleConverterNotFound(GenericConversionService.java:321) ~[spring-core-5.0.11.RELEASE.jar:5.0.11.RELEASE]
	at org.springframework.core.convert.support.GenericConversionService.convert(GenericConversionService.java:194) ~[spring-core-5.0.11.RELEASE.jar:5.0.11.RELEASE]
	at org.springframework.boot.context.properties.bind.BindConverter$CompositeConversionService.convert(BindConverter.java:162) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.BindConverter.convert(BindConverter.java:96) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.BindConverter.convert(BindConverter.java:88) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.MapBinder.bindAggregate(MapBinder.java:67) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.AggregateBinder.bind(AggregateBinder.java:58) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.lambda$bindAggregate$2(Binder.java:305) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder$Context.withIncreasedDepth(Binder.java:441) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder$Context.access$100(Binder.java:381) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.bindAggregate(Binder.java:304) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.bindObject(Binder.java:262) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	at org.springframework.boot.context.properties.bind.Binder.bind(Binder.java:221) ~[spring-boot-2.0.7.RELEASE.jar:2.0.7.RELEASE]
	... 21 common frames omitted



```

抓住关键信息:
```

 Failed to bind properties under 'logging.level'

```


错误产生背景：
SpringBoot1.5.9升级为2.0.7版本。


错误原因分析:
是因为SpringBoot2.0以上版本日志需要指定包路径才行。


解决办法(修改application.yml配置文件):

原文件关键内容如下:
```
logging:
  level: warn

```

将其改为(指定包路径):
```
logging:
   com.blog.springboot: WARN

```

com.blog.springboot是我个人博客的启动类，你们可以将其改为你自己的即可。
最后问题迎刃而解。