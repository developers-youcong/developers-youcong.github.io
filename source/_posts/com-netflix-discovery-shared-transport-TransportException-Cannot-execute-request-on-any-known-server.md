---
title: >-
 com.netflix.discovery.shared.transport.TransportException: Cannot executerequest on any known server
date: 2020-11-07 11:20:07
tags: "SpringCloud"
---

#### 1.错误信息
```
com.netflix.discovery.shared.transport.TransportException: Cannot execute request on any known server

```
<!--more-->
#### 2.错误背景

启动Eureka Server报错

#### 3.错误原因

Spring2.0以后默认开的安全验证，你需要手动关闭，关闭方法为添加一个配置方法。

#### 4.解决办法

##### (1)添加Maven依赖
```
        <dependency>
	        <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

##### (2)添加配置类

```
package com.springcloud.blog.config;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // Configure HttpSecurity as needed (e.g. enable http basic).
 http.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.NEVER);
        http.csrf().disable();
        
        //注意：为了可以使用 http://u s e r : {user}:user:{password}@h o s t : {host}:host:{port}/eureka/ 这种方式登录,所以必须是httpBasic,
        
        // 如果是form方式,不能使用url格式登录     http.authorizeRequests().anyRequest().authenticated().and().httpBasic();

    }
}

```

##### (3)完成(1)和(2)步骤，重启即可

参考资料如下:

[com.netflix.discovery.shared.transport.TransportException: Cannot execute request on any known serve](https://blog.csdn.net/luansha0/article/details/88789742)



#### 5.补充说明

最奇葩的是，我后来尝试去除该配置也能正常启动Eureka Server成功,之前早上启动的时候一直报错，哪怕退出IDE重新登录也一样，同时也验证过配置文件是否有问题，但都没有解决，于是找到了上述的链接，加了maven依赖和配置类，一启动就成功了。

