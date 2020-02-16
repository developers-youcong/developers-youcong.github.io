---
title: SpringBoot之文件上传体积过大问题(解决方案)
date: 2019-09-19 17:41:47
tags: "SpringBoot"
---

错误信息如下(关键):
```
org.apache.tomcat.util.http.fileupload.FileUploadBase$SizeLimitExceededException: the request was rejected because its size (110862330) exceeds the configured maximum (31457280)


```
<!--more-->

解决方案(主要是修改application.yml对应的配置):
如果觉得300MB不够的话，可以往上调。

```
spring:
  http:
    multipart:
      enabled: true
      max-file-size: 300MB 
      max-request-size: 300MB

```