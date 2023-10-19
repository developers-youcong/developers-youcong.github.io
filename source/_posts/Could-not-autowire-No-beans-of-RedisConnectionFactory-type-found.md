---
title: Could not autowire. No beans of 'RedisConnectionFactory' type found
date: 2023-10-17 20:41:31
tags: "Redis"
---


SpringBoot版本2.7.2增加Redis配置类，其中如下代码IDE报红线：
<!--more-->
```
RedisConnectionFactory connectionFactory

```

红线显示的错误信息如下:
```
Could not autowire. No beans of 'RedisConnectionFactory' type found.

```

这个错误信息很奇怪，后来搜索了解,原来是因为SpringBoot高版本导致的，需要降低版本，或者如果不想降低版本，可以通过加注解解决。实际上，不论这个代码加不加注解，都不影响SpringBoot2.7.2版本使用Redis(可能从程序员角度来看，报红线总归是不好的，除此外，也没有其它影响)。

严格意义上说，这不算是错误信息，我所理解的错误信息，一定是影响软件系统整体或局部功能服务的。

也许后来朋友会遇到这样的问题，我针对此的解决办法归纳为二个方面：

- 1.降低SpringBoot版本，低于2.7.0以下，就不会再出现红线(来源于网上解决方案)。
- 2.在配置类对应的方法上添加注解 @ConditionalOnSingleCandidate(实际上添不添加注解，都不影响正常使用)。

**添加注解方案：**
```
@Configuration
@EnableCaching
public class RedisConfig extends CachingConfigurerSupport {
    @Bean
    @ConditionalOnSingleCandidate //(添加这个注解)
    @SuppressWarnings(value = {"unchecked", "rawtypes", "deprecation"})
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);

        FastJson2JsonRedisSerializer serializer = new FastJson2JsonRedisSerializer(Object.class);

        ObjectMapper mapper = new ObjectMapper();
        mapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        mapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        serializer.setObjectMapper(mapper);

        template.setValueSerializer(serializer);
        // 使用StringRedisSerializer来序列化和反序列化redis的key值
        template.setKeySerializer(new StringRedisSerializer());
        template.afterPropertiesSet();
        return template;
    }
}

```

