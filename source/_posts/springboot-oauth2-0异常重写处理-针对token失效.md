---
title: springboot+oauth2.0异常重写处理(针对token失效)
date: 2020-08-23 10:35:30
tags: "SpringBoot"
---

近来针对微服务框架开发，其中oauth2.0默认返回XML形式的token失效，不符合我们实际的开发需求，于是我参考网上一些博客重写了它,使其符合我们开发的需求。
<!--more-->

核心主要涉及两个类:
```
import com.eqics.common.security.utils.ResultJsonUtil;
import org.springframework.http.HttpStatus;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.oauth2.common.exceptions.InvalidTokenException;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.stereotype.Component;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
@Component
public class AuthExceptionEntryPoint implements AuthenticationEntryPoint {


    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response,
                         AuthenticationException authException) throws ServletException {
        Map<String, Object> map = new HashMap<String, Object>();
        Throwable cause = authException.getCause();

        response.setStatus(HttpStatus.OK.value());
        response.setHeader("Content-Type", "application/json;charset=UTF-8");
        try {
            if (cause instanceof InvalidTokenException) {

                response.getWriter().write(ResultJsonUtil.build(
                        222222,
                        "token失效"
                ));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```


ResourceServerConfig.java类中补充如下(找到主要方法):
```
    @Override
    public void configure(ResourceServerSecurityConfigurer resources) {
        resources.tokenServices(tokenServices());
        resources.authenticationEntryPoint(new AuthExceptionEntryPoint());

    }

```

还有一个工具类ResultJsonUtil.java，内容如下:
```
import java.util.List;
import java.util.Map;
public class ResultJsonUtil<T> {

    private int code;
    private int statusCode;
    private String msg;
    private T data;

    private static final int DEFAULT_STATUS_CODE = 0;

    /**
     * construction
     *
     * @param code       请求状态码
     * @param statusCode 信息状态码
     * @param msg        信息
     * @param data       数据
     */
    public ResultJsonUtil(int code, int statusCode, String msg, T data) {
        this.code = code;
        this.statusCode = statusCode;
        this.msg = msg;
        this.data = data;
    }

    public static String build(int code, int statusCode, String msg) {
        ResultJsonUtil<String> resultJsonUtil = new ResultJsonUtil<>(code, statusCode, msg, "");
        return resultJsonUtil.getResultJson();
    }

    public static String build(int code, String msg) {
        return ResultJsonUtil.build(code, ResultJsonUtil.DEFAULT_STATUS_CODE, msg);
    }

    public static String build(int code, int statusCode, String msg, JSONArray data) {
        ResultJsonUtil<JSONArray> resultJsonUtil = new ResultJsonUtil<>(code, statusCode, msg, data);
        return resultJsonUtil.getResultJson();
    }

    public static String build(int code, String msg, JSONArray data) {
        return ResultJsonUtil.build(code, ResultJsonUtil.DEFAULT_STATUS_CODE, msg, data);
    }


    public static String build(int code, int statusCode, String msg, Map data) {
        JSONObject jsonObjectData = JSONObject.parseObject(JSON.toJSONString(data));
        ResultJsonUtil<JSONObject> resultJsonUtil = new ResultJsonUtil<>(code, statusCode, msg, jsonObjectData);
        return resultJsonUtil.getResultJson();
    }

    public static String build(int code, String msg, Map data) {
        return ResultJsonUtil.build(code, ResultJsonUtil.DEFAULT_STATUS_CODE, msg, data);
    }


    public static String build(int code, int statusCode, String msg, List data) {
        JSONArray jsonArrayData = JSONArray.parseArray(JSON.toJSONString(data));
        return ResultJsonUtil.build(code, statusCode, msg, jsonArrayData);
    }

    public static String build(int code, String msg, List data) {
        return ResultJsonUtil.build(code, ResultJsonUtil.DEFAULT_STATUS_CODE, msg, data);
    }

    private String getResultJson() {
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("code", this.code);
        jsonObject.put("msg", this.msg);
        return JSON.toJSONString(jsonObject, SerializerFeature.DisableCircularReferenceDetect);
    }
}


```

本文主要参考了这篇文章:
[Spring Cloud：Security OAuth2 自定义异常响应](https://www.cnblogs.com/bndong/p/10275430.html)
