---
title: Springboot实现跨域请求
date: 2019-03-07 20:12:29
tags: "SpringBoot"
---
之所以需要用到跨域请求，目的在于现在的Java项目，几乎基本上都前后端分离，除一些较老的维护项目外(通常是单体或者是maven多模块形式，不过本质上还是将前端放在webapps下)。

SpringBoot实现跨域其实和Spring是一样，区别不大，如果要说区别的话，Spring需要在对应的xml文件中配置bean，而SpringBoot则通过注解的方式。

Spring配置跨域请求可参考我的这篇文章:https://www.cnblogs.com/youcong/p/9676433.html

<!--more-->
示例代码如下:
```
package com.blog.springboot.config;

import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.cors.CorsConfiguration;  
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;  
import org.springframework.web.filter.CorsFilter;  

@Configuration  
public class CorsConfig {  
    private CorsConfiguration buildConfig() {  
        CorsConfiguration corsConfiguration = new CorsConfiguration();  
        corsConfiguration.addAllowedOrigin("*"); // 1允许任何域名使用
        corsConfiguration.addAllowedHeader("*"); // 2允许任何头
        corsConfiguration.addAllowedMethod("*"); // 3允许任何方法（post、get等） 
        return corsConfiguration;  
    }  
  
    @Bean  
    public CorsFilter corsFilter() {  
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();  
        source.registerCorsConfiguration("/**", buildConfig()); // 4  
        return new CorsFilter(source);  
    }  
}
```

本文主要参考链接为:
SpringBoot跨域配置:https://www.cnblogs.com/nananana/p/8492185.html