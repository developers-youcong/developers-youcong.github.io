---
title: Hutool之Http工具类使用
date: 2020-11-21 13:35:37
tags: "Java"
---

最早接触Hutool这个集常用工具类为一体的框架工具包是在2018年的时候(酒店业务需要调用第三方门锁API)。
而后19年因为业务接触到Bmob云，开始写对Bmob云的API，于是便有了这篇文章[Hutool工具类之HttpUtil使用Https](https://www.cnblogs.com/youcong/p/10809078.html)

最近针对业务，再次用到这个。这次涉及到不同单个服务之间的调用，通信方式还是HTTP为主。

针对最近常用的，做了一些总结。
<!--more-->

## 二、应用场景
- (1)调用第三方服务API(第三方服务通常支持HTTP、WebService，一般HTTP比较多);
- (2)单体应用服务之间的服务调用;
- (3)分布式服务之间的服务调用;

## 二、HttpUtil

官方文档地址:
https://hutool.cn/docs/#/http/Http%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%B7%A5%E5%85%B7%E7%B1%BB-HttpUtil

大家可以做个参考。

### 1.Get请求

```
 String apiData = HttpUtil.get(url);
 
 String apiData = HttpUtil.get(url,timeout);
 
 String apiData = HttpUtil.get(url,paramMap);
 
 String apiData = HttpUtil.get(url,paramMap,timeout);
 
 String apiData = HttpUtil.createGet(url)
                .execute().body();
```

### 2.Post请求
```
String apiData = HttpUtil.post(url,body);

String apiData = HttpUtil.post(url,body,timeout);

String apiData =HttpUtil.post(url,paramMap,timeout);

String apiData =HttpUtil.post(url,paramMap);

String apiData = HttpUtil.createPost(url)
        .body(reqDto.toString())
        .execute().body();
```

至于apiData如何由String转成JSON格式化，可通过Hutools自带的JSONObject对象或者JSONArray对象进行转换。

例子如下(以我调用博客园API为例):
```
 private String getToken() {
        String url = "https://api.cnblogs.com/token";//请求接口地址
        Map<String, Object> paramMap = new HashMap<>();
        paramMap.put("client_id", ClientId);
        paramMap.put("client_secret", ClientSecret);
        paramMap.put("grant_type", "client_credentials");
        String result = HttpUtil.post(url, paramMap);

        JSONObject jsonObject = new JSONObject(result);

        return "Bearer " + jsonObject.getStr("access_token");

    }


```
上面是针对单一JSONObject对象，下面还有针对数组是如何拿到具体的元素(这里是我对接博客园API代码，拿到博客园各个博主的名称，然后根据名称去匹配URL，实现批量数据抓取):
```
 try {

            Integer pageMaxSize = 200;

            Integer pageSize = 30;

            for (int pageParentIndex = 0; pageParentIndex < pageMaxSize; pageParentIndex++) {
				
                String homeApiPageData = cnBlogApiService.getSiteHomePostList(pageParentIndex, pageSize);

                JSONArray getHomeApiPageData = new JSONArray(homeApiPageData);


                if (getHomeApiPageData.size() > 0) {

                    for (int i = 1; i < getHomeApiPageData.size(); i++) {


                        JSONObject jsonObject = getHomeApiPageData.getJSONObject(i);

                        this.executeCnBlogsImportDataTask(jsonObject.getStr("BlogApp"));
                    }

                }
            }

            return ResponseBaseDTO.createSuccResp(1);
        } catch (Exception e) {
            logger.error("/cnblogs/singleImport", e);
            return ResponseBaseDTO.createFailResp(e.getMessage());
        }

```
