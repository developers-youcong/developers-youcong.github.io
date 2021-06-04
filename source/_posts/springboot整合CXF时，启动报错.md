---
title: springboot整合CXF时，启动报错
date: 2020-10-25 20:16:05
tags: "SpringBoot"
---

错误信息:
```
***************************
APPLICATION FAILED TO START
***************************
Description:
Parameter 1 of constructor in org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration required a bean of type 'org.springframework.boot.autoconfigure.web.servlet.DispatcherServletPath' that could not be found.
The following candidates were found but could not be injected:
    - Bean method 'dispatcherServletRegistration' in 'DispatcherServletAutoConfiguration.DispatcherServletRegistrationConfiguration' not loaded because DispatcherServlet Registration found non dispatcher servlet dispatcherServlet
Action:
Consider revisiting the entries above or defining a bean of type 'org.springframework.boot.autoconfigure.web.servlet.DispatcherServletPath' in your configuration.

```
<!--more-->
通过搜索，说是和SpringBoot版本冲突导致的。

我的SpringBoot版本为2.2.6.RELEASE

整合的Apache CXF版本为3.2.4

我的CXF配置类代码有一段这样的:
```
@Bean
    public ServletRegistrationBean dispatcherServlet() {
        return new ServletRegistrationBean(new CXFServlet(), "/cxf/*");// 发布服务名称 localhost:8080/cxf
    }

```

我将其去除并在application.yml配置这行代码:
```
cxf:
  path: /cxf  

```

最终项目启动起来了，问题得到解决。