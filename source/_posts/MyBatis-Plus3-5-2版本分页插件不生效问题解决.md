---
title: MyBatis-Plus3.5.2版本分页插件不生效问题解决
date: 2023-08-03 22:33:42
tags: "MyBatis-Plus"
---

解决问题代码:
<!--more-->
```

@EnableTransactionManagement
@Configuration
@MapperScan("com.framework.fs.mapper")
public class MyBatisPlusConfig {
    /**
     * 分页插件
     */
    @Bean
    public PaginationInnerInterceptor paginationInterceptor() {
        PaginationInnerInterceptor page = new PaginationInnerInterceptor();
        page.setMaxLimit(-1L);
        page.setDbType(DbType.MYSQL);
        page.setOptimizeJoin(true);
        return page;
    }

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        mybatisPlusInterceptor.setInterceptors(Collections.singletonList(paginationInterceptor()));
        return mybatisPlusInterceptor;
    }


}


```