---
title: 从零开始学YC-Framework之Knife4j
date: 2022-08-21 14:35:56
tags: "YC-Framework"
---

## 一、Knife4j是什么？
Knife4j是一个集Swagger2 和 OpenAPI3为一体的增强解决方案。
<!--more-->

官方网站：
https://doc.xiaominfo.com/

官方文档：
https://doc.xiaominfo.com/docs/quick-start

Github源代码：
https://github.com/xiaoymin/swagger-bootstrap-ui

## 二、为什么要使用Knife4j？
使用Knife4j是为了简化内部对接，便于测试和前端调试，以便开发人员或其他专门人员编写接口文档。

## 三、YC-Framework中是如何使用Knife4j的？
YC-Framework将Knife4j模块化，相关微服务模块只需引入和配置就能使用Knife4j所提供的功能特性。

**yc-common-knife4j模块核心代码：**

```
@Configuration
@EnableSwagger2WebMvc
public class Knife4jConfiguration {

    @Value("${spring.application.name}")
    private String applicationName;

    private static final String URL = "http://www.ycframework.com/";

    private static final String AUTHOR_EMAIL = "youcongtech@163.com";

    private static final String VERSION = "1.0";

    private static final String GROUP_NAME = "1.0版本";

    @Bean(value = "api")
    public Docket groupRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(groupApiInfo())
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo groupApiInfo(){
        return new ApiInfoBuilder()
                .title(getTitle(applicationName))
                .description(getDesc(applicationName))
                .termsOfServiceUrl(URL)
                .contact(AUTHOR_EMAIL)
                .version(VERSION)
                .build();
    }


    @Bean(value = "defaultApi2")
    public Docket defaultApi2() {

        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(new ApiInfoBuilder()
                        .title(getTitle(applicationName))
                        .description(getDesc(applicationName))
                        .termsOfServiceUrl(URL)
                        .contact(AUTHOR_EMAIL)
                        .version(VERSION)
                        .build())
                //分组名称
                .groupName(GROUP_NAME)
                .select()
                //这里指定Controller扫描包路径
                .apis(RequestHandlerSelectors.basePackage(getBasePackage(applicationName)))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }

    /**
     * 包扫描自定义
     *
     * @param applicationName
     * @return
     */
    private String getBasePackage(String applicationName) {
        Console.log("applicationName:" + applicationName);
        String basePackage = "";
        switch (applicationName) {
            case ApplicationConst.AUTH:
                basePackage = ApplicationConst.KNIFE4J_AUTH_PACKAGE;
                break;
            case ApplicationConst.ADMIN:
                basePackage = ApplicationConst.KNIFE4J_ADMIN_PACKAGE;
                break;
            case ApplicationConst.CMS:
                basePackage = ApplicationConst.KNIFE4J_CMS_PACKAGE;
                break;
            case ApplicationConst.CRAWLER:
                basePackage = ApplicationConst.KNIFE4J_CRAWLER_PACKAGE;
                break;
            case ApplicationConst.FILE:
                basePackage = ApplicationConst.KNIFE4J_FILE_PACKAGE;
                break;
            case ApplicationConst.PLUGINS:
                basePackage = ApplicationConst.KNIFE4J_PLUGINS_PACKAGE;
                break;
            case ApplicationConst.WECHAT:
                basePackage = ApplicationConst.KNIFE4J_WECHAT_PACKAGE;
                break;
            default:
                basePackage = ApplicationConst.DEFAULT_PACKAGE;
                break;
        }
        Console.log("basePackage:" + basePackage);
        return basePackage;
    }

    /**
     * 服务名称
     *
     * @param applicationName
     * @return
     */
    private String getTitle(String applicationName) {
        Console.log("applicationName:" + applicationName);
        String title = "";
        switch (applicationName) {
            case ApplicationConst.AUTH:
                title = "统一认证服务";
                break;
            case ApplicationConst.ADMIN:
                title = "统一用户管理";
                break;
            case ApplicationConst.CMS:
                title = "统一内容管理";
                break;
            case ApplicationConst.CRAWLER:
                title = "统一爬虫管理";
                break;
            case ApplicationConst.FILE:
                title = "统一文件管理";
                break;
            case ApplicationConst.PLUGINS:
                title = "统一插件管理";
                break;
            case ApplicationConst.WECHAT:
                title = "微信生态管理";
                break;
            default:
                title = "默认应用管理";
                break;
        }
        Console.log("title:" + title);
        return title;
    }

    /**
     * 描述
     *
     * @param applicationName
     * @return
     */
    private String getDesc(String applicationName) {
        Console.log("applicationName:" + applicationName);
        String desc = "";
        switch (applicationName) {
            case ApplicationConst.AUTH:
                desc = "Auth API";
                break;
            case ApplicationConst.ADMIN:
                desc = "User API";
                break;
            case ApplicationConst.CMS:
                desc = "CMS API";
                break;
            case ApplicationConst.CRAWLER:
                desc = "Crawler API";
                break;
            case ApplicationConst.FILE:
                desc = "File API";
                break;
            case ApplicationConst.PLUGINS:
                desc = "Plugins API";
                break;
            case ApplicationConst.WECHAT:
                desc = "Wechat API";
                break;
            default:
                desc = "Default API";
                break;
        }
        Console.log("desc:" + desc);
        return desc;
    }
}


```

YC-Framework相关微服务使用knife4j按照如下步骤执行即可！！！

### 1.引入模块依赖
```
<dependency>
    <groupId>com.yc.framework</groupId>
    <artifactId>yc-common-knife4j</artifactId>
</dependency>

```

### 2.按照规则配置Knife4jConfiguration类即可

### 3.可在任意Controller中使用Knife4j注解

**相关微服务示例如下:**

https://github.com/developers-youcong/yc-framework/tree/main/yc-auth


https://github.com/developers-youcong/yc-framework/tree/main/yc-modules/yc-admin


以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star！！！


YC-Framework官网：
https://framework.youcongtech.com/


YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework


YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework