---
title: 'java.lang.IllegalStateException: Service id not legal hostname (/test-gw-aqa)'
date: 2020-09-05 15:24:40
tags: "SpringCloud"
---

错误信息:
```
java.lang.IllegalStateException: Service id not legal hostname (/test-gw-aqa)

```

错误原因和解决方案:

FeignClient错误写法:
```
@FeignClient("/test-gw-aqa")

```


FeignClient正确写法:
```
@FeignClient("test-gw-aqa")

```


这种错误会直接导致微服务之间调用失败，因为找不到这个微服务。

