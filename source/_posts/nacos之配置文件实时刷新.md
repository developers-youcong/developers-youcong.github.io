---
title: nacos之配置文件实时刷新
date: 2020-11-30 20:51:37
tags: "SpringCloud"
---

当初为了解决nacos配置文件实时刷新问题，搜索了很多资料，仍无效，最后不经意间的尝试却解决了这个问题。

我的SpringCloud版本为:Hoxton.SR4；

我的SpringCloud Alibaba版本为:2.2.1.RELEASE；

我的Nacos版本为:1.3.1。
<!--more-->
### 一、核心配置文件(一定要是bootstrap.yml，而非application.yml)
```
  cloud:
    nacos:
      discovery:
        # 服务注册地址
        server-addr: 127.0.0.1:8848
      config:
        # 配置中心地址
        server-addr: 127.0.0.1:8848
        # 配置文件格式
        file-extension: yml
        # 共享配置
        shared-dataids: application-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension},job.yml,test.properties
        refresh-enabled: true
        refreshable-dataids: job.yml,test.properties

```

上面是完整的，最关键的是如下两行:
```
refresh-enabled: true
refreshable-dataids: job.yml,test.properties

```

### 二、读取配置文件的类上，需加上@RefreshScope注解
```
@Component
@RefreshScope
@RestController
public class TestConfigJob {


    @Value("${job.live.cron}")
    private String val;

    @Autowired
    private Environment env;

    @Autowired
    private EqicsSaasApiProperties eqicsSaasApi;

    @Scheduled(cron = "1 * * * * ?")
    public void test() {
        System.out.println(DateUtil.date() + " val:" + val);

        System.out.println("api_url:" + eqicsSaasApi.getApiUrl());
    }

    @GetMapping("/getConfig")
    public String getConfig() {

        Map<String, Object> returnMap = new HashMap<>();
        returnMap.put("val", val);
        return JSONUtil.parse(returnMap).toString();

    }
}


```


