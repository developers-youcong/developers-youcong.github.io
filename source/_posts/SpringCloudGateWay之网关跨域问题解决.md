---
title: SpringCloudGateWay之网关跨域问题解决
date: 2020-08-29 21:53:49
tags: "SpringCloud"
---
应用场景:
所有的微服务是通过网关这个入口，在和前端对接口时，必然设计到有关跨域的问题。关于服务端跨域有很多方案，可以加注解(指定具体的路径允许跨域)，也可以统一配置。

<!--more-->
另外如果不在网关入口这配置，势必会造成一个很大的影响，那就是前端通过网关入口调用其它微服务，通常会出现如下错误:
```
Access to XMLHttpRequest at 'xxx' from origin 'xxx' has been been blocked by CORS policy

```

核心代码(解决方案):
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.reactive.CorsWebFilter;
import org.springframework.web.cors.reactive.UrlBasedCorsConfigurationSource;
import org.springframework.web.util.pattern.PathPatternParser;

@Configuration
public class CorsConfig {

    @Bean
    public CorsWebFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource(new PathPatternParser());
        source.registerCorsConfiguration("/**", buildConfig());
        return new CorsWebFilter(source);
    }

    private CorsConfiguration buildConfig() {
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        //在生产环境上最好指定域名，以免产生跨域安全问题
        corsConfiguration.addAllowedOrigin("*");
        corsConfiguration.addAllowedHeader("*");
        corsConfiguration.addAllowedMethod("*");
        return corsConfiguration;
    }
}

```

SpirngCloud GateWay解决方案:
[Spring Cloud Gateway -- Cors解决跨域问题](https://blog.csdn.net/a294634473/article/details/90715903)


注解解决方案:
[java后端解决跨域问题（过滤器或者注解）](https://blog.csdn.net/weixin_41796956/article/details/84133901)
