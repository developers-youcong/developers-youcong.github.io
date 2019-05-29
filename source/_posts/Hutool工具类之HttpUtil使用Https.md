---
title: Hutool工具类之HttpUtil使用Https
date: 2019-04-30 11:09:57
tags: "Java"
---

关于Hutool工具类之HttpUtil如何使用可以参考官方文档[Hutool之HttpUtil](https://www.hutool.cn/docs/#/http/Http%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%B7%A5%E5%85%B7%E7%B1%BB-HttpUtil)

其实使用Http和Https使用的方式是一样的。

建议大家可以看看HttpUtil的源码，感觉设计的挺不错的。
<!--more-->

## 导入Maven依赖
```
    <dependency>
		    <groupId>cn.hutool</groupId>
		    <artifactId>hutool-all</artifactId>
		    <version>4.1.0</version>
	  </dependency>
	  

```


## 编写测试类(使用Junit单元测试)
```
    @Test
	public void testHttps() throws Exception {
		
		JSONObject json = new JSONObject();
		json.put("username", "1332788xxxxxx");
		json.put("password", "123456.");
		
		String result = HttpRequest.post("https://api2.bmob.cn/1/users")
				.header("Content-Type", "application/json")
				.header("X-Bmob-Application-Id","2f0419a31f9casdfdsf431f6cd297fdd3e28fds4af")
				.header("X-Bmob-REST-API-Key","1e03efdas82178723afdsafsda4be0f305def6708cc6")
			    .body(json)
			    .execute().body();
		
		
        System.out.println(result);
				
				
	}

```
方法解释(上面采用的是一种叫链式编程的方式):
header对应的是请求头。
body对应的是请求体(包含参数和参数值)。
HttpRequest里面包含Post、GET、Delete、Put等常用的RestFul方式。


打印如下:
```
{"createdAt":"2019-04-30 10:42:07","objectId":"6cfdb77081","sessionToken":"269e433440c9e65b8058d016df703ccb"}

```