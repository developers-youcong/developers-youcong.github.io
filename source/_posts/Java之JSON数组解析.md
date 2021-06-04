---
title: Java之JSON数组解析
date: 2020-06-15 21:38:31
tags: "Java"
---
最近打通博客园相关API和其它第三方API，由于我开发的系统内部需要调用较多的第三方API，所以用到了SpringCloud中的Feign。
<!--more-->
由于之前开发的系统基本上除了支付是用的微信或支付宝以及智能门锁用的是第三方之外，其它很少涉及。

今天在做迁移博客数据的时候发现，通过Feign调用第三方时解析数组出现问题。

出现这样类似的错误，如下:
```
A JSONObject text must begin with '{' at character 1 of 1

```

问题原因是因为我所返回的数据是这样的格式[{}],例如:
```
[
    {
        "Id": 13086998,
        "Title": "SLF4J: Class path contains multiple SLF4J bindings.",
        "Url": "http://www.cnblogs.com/youcong/p/13086998.html",
        "Description": "错误信息: SLF4J Warning: Class Path Contains Multiple SLF4J Bindings 错误原因:我个人博客系统一个爬虫组件用到webmagic，而webmagic与lomback中的slf有冲突。 解决办法(webmagic排除相关依赖即可): <!-- ",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-10T21:15:00",
        "ViewCount": 11,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13086991,
        "Title": "SpringCloud之Security",
        "Url": "http://www.cnblogs.com/youcong/p/13086991.html",
        "Description": "Spring Security是Spring提供的一个安全框架，提供认证和授权功能，最主要的是它提供了简单的使用方式，同时又有很高的灵活性，简单，灵活，强大。 我个人博客系统采用的权限框架就是Spring Security，正好整合到SpringCloud里面。一般系统里关于角色方面通常有这么几张表",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-10T21:14:00",
        "ViewCount": 150,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13066768,
        "Title": "SpringCloud之Config",
        "Url": "http://www.cnblogs.com/youcong/p/13066768.html",
        "Description": "配置中心，也就是SpringCloud中的Config组件，主要应用在哪些方面? 配置文件方便维护 配置文件内容安全和权限 更新项目配置不需要重启 本文主要围绕两个方面，一个是Config Server，另一个是Config Client。还是以我个人博客系统其中的一个模块为例。 一、搭建Confi",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T21:07:00",
        "ViewCount": 8,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13066734,
        "Title": "SpringCloud之Zuul",
        "Url": "http://www.cnblogs.com/youcong/p/13066734.html",
        "Description": "使用SpringCloud Zuul实现网关代理。 一、Maven依赖 <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-netflix-eureka-client</",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T21:06:00",
        "ViewCount": 19,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13066804,
        "Title": "SpringCloud之Ribbon",
        "Url": "http://www.cnblogs.com/youcong/p/13066804.html",
        "Description": "SpringCloud通过Ribbon实现负载均衡。 一、添加Maven依赖 <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-netflix-eureka-clien",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T21:05:00",
        "ViewCount": 10,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13066706,
        "Title": "SpringCloud之Hystrix",
        "Url": "http://www.cnblogs.com/youcong/p/13066706.html",
        "Description": "在微服务架构中，微服务之间互相依赖较大，相互之间调用必不可免的会失败。但当下游服务A因为瞬时流量导致服务崩溃，其他依赖于A服务的B、C服务由于调用A服务超时耗费了大量的资源，长时间下去，B、C服务也会崩溃。Hystrix就是用来解决服务之间相互调用失败，避免产生蝴蝶效应的熔断器，以及提供降级选项。H",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T21:04:00",
        "ViewCount": 14,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13066686,
        "Title": "SpringCloud之Feign",
        "Url": "http://www.cnblogs.com/youcong/p/13066686.html",
        "Description": "以我个人写的博客系统为例，请求其它微服务API。 一、添加Maven依赖 <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-openfeign</artifactId>",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T21:03:00",
        "ViewCount": 8,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13042794,
        "Title": "SpringCloud之服务注册中心和提供者(Eureka Server和Eureka Client)",
        "Url": "http://www.cnblogs.com/youcong/p/13042794.html",
        "Description": "一、使用Eureka Server搭建服务注册中心 1.Maven依赖 <dependencies> <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-netflix-",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T21:01:00",
        "ViewCount": 17,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13027193,
        "Title": "GitHub Pages Hexo 配置来自阿里云的域名或腾讯云的域名",
        "Url": "http://www.cnblogs.com/youcong/p/13027193.html",
        "Description": "参考地址如下(亲试有效，我的博客应该试过了，可以指向我的域名，之所以我没有指向是因为博客的阅读量和访问等目前不能迁移所以就不做指向了):Github个人博客：绑定域名 腾讯云GitHub Pages Hexo 配置来自阿里云的域名",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T20:59:00",
        "ViewCount": 11,
        "CommentCount": 0,
        "DiggCount": 0
    },
    {
        "Id": 13066658,
        "Title": "博客园开放API如何使用",
        "Url": "http://www.cnblogs.com/youcong/p/13066658.html",
        "Description": "业务背景:我通过weblogic这个Java爬虫框架是能够爬取得到博客园的大多数数据，但后来得知博客园有自己的开放API，通过这个开放API可以做一些事情，比方说实现一个关于博客园文章的小程序阅读、或者想学习go、node.js、flutter或uniapp用其实现一个CMS应用。 一、API KE",
        "Author": "挑战者V",
        "BlogApp": "youcong",
        "Avatar": "https://pic.cnblogs.com/face/1255290/20190518211156.png",
        "PostDate": "2020-06-08T20:55:00",
        "ViewCount": 25,
        "CommentCount": 0,
        "DiggCount": 0
    }
]

```

直接用JSONObject解析是不行的，需要用JSONArray去做。

解决问题核心代码:
```
  String result = cnBlogApiService.getPersonalBlogPostList(pageIndex, accessToken); //取第三方API返回的JSON数据


        JSONArray jsonArray = new JSONArray(result);
        
        if(jsonArray.size()>0){
            for(int i = 0 ;i<jsonArray.size();i++){
                JSONObject jsonObject =jsonArray.getJSONObject(i);
                System.out.println("js:"+jsonObject.getStr("Url"));
            }
        }

```




