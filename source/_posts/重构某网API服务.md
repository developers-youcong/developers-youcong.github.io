---
title: 重构某网API服务
date: 2022-03-17 19:22:16
tags: "系统重构"
---
本文主要内容:

- 1.背景；
- 2.重构的方式；
- 3.总结。


<!--more-->

## 一、背景
某网API微服务一直持续稳定的运行，某网一直向其稳定地推送数据。关于这一点内容，我在[我在M2公司做架构之某网与Webservice](https://youcongtech.com/2021/10/30/%E6%88%91%E5%9C%A8M2%E5%85%AC%E5%8F%B8%E5%81%9A%E6%9E%B6%E6%9E%84%E4%B9%8B%E6%9F%90%E7%BD%91%E4%B8%8EWebservice/)提到过，这里不再赘述。某网API鉴权我一直觉得非常冗余，有不少地方没有必要，可以更简化。注意，简化不等同于随意或降低安全性。关于安全性相关的，以下文章叙述的非常多，感兴趣的朋友可以阅读(有理论也有实战):
[WebService安全机制的思考与实践](https://youcongtech.com/2020/10/31/WebService%E5%AE%89%E5%85%A8%E6%9C%BA%E5%88%B6%E7%9A%84%E6%80%9D%E8%80%83%E4%B8%8E%E5%AE%9E%E8%B7%B5/)
[服务器安全策略之思考与实践](https://youcongtech.com/2021/07/16/%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%89%E5%85%A8%E7%AD%96%E7%95%A5%E4%B9%8B%E6%80%9D%E8%80%83%E4%B8%8E%E5%AE%9E%E8%B7%B5/)
[我在M2公司做架构之OAuth2-0](https://youcongtech.com/2021/10/17/%E6%88%91%E5%9C%A8M2%E5%85%AC%E5%8F%B8%E5%81%9A%E6%9E%B6%E6%9E%84%E4%B9%8BOAuth2-0/)

## 二、重构的方式
- 1.基于Sa-Token进行重构；
- 2.基于拦截器进行重构。

### 1.基于Sa-Token进行重构

#### (1)导入Maven依赖
```
<properties>
    <sa-token-spring-boot-starter.version>1.28.0</sa-token-spring-boot-starter.version>
</properties>

<dependencies>
<!-- Sa-Token 权限认证, 在线文档：http://sa-token.dev33.cn/ -->
<dependency>
    <groupId>cn.dev33</groupId>
    <artifactId>sa-token-spring-boot-starter</artifactId>
    <version>${sa-token-spring-boot-starter.version}</version>
    </dependency>
</dependencies>

```

#### (2)bootstrap.yml配置
```
# Sa-Token配置
sa-token:
  # token名称 (同时也是cookie名称)
  token-name: satoken
  # token有效期，单位s 默认30天, -1代表永不过期
  timeout: 2592000
  # token临时有效期 (指定时间内无操作就视为token过期) 单位: 秒
  activity-timeout: -1
  # 是否允许同一账号并发登录 (为true时允许一起登录, 为false时新登录挤掉旧登录)
  is-concurrent: true
  # 在多人登录同一账号时，是否共用一个token (为true时所有登录共用一个token, 为false时每次登录新建一个token)
  is-share: false
  # token风格
  token-style: uuid
  # 是否输出操作日志
  is-log: false

```

#### (3)编写配置类
```
@Configuration
public class SaTokenConfigure implements WebMvcConfigurer {
    // 注册Sa-Token的注解拦截器，打开注解式鉴权功能
    // 注册拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 注册Sa-Token的路由拦截器，并排除登录接口或其他可匿名访问的接口地址 (与注解拦截器无关)
        registry.addInterceptor(new SaRouteInterceptor()).addPathPatterns("/**").excludePathPatterns(
                "/xxnet/token", "/xxnet/isLogin", "/doc.html", "/webjars/**", "/swagger-resources", "/v2/**");
    }
}

```

### 2.基于拦截器进行重构
核心代码如下:
```
@Component
public class AuthInterceptor implements HandlerInterceptor {

    /**
     * 忽略拦截的url
     */
    private String urls[] = {
            "/xxnet/token"
    };

    /**
     * 进入controller层之前拦截请求
     *
     * @param httpServletRequest
     * @param httpServletResponse
     * @param o
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        String url = httpServletRequest.getRequestURI();
        String username = httpServletRequest.getHeader("username");
        String password = httpServletRequest.getHeader("password");

        // 遍历需要忽略拦截的路径
        for (String item : this.urls) {
            if (item.equals(url)) {
                return true;
            }
        }

        if (username == null || password == null) {
            httpServletResponse.setCharacterEncoding("UTF-8");
            httpServletResponse.setContentType("application/json; charset=utf-8");
            PrintWriter out = null;
            ResultBody res = new ResultBody(ResultCode.TOKEN_ERROR.getCode(), ResultCode.TOKEN_ERROR.getMsg());
            res.setMsg("用户名或密码不能为空");
            res.setCode("400");
            String json = JSONUtil.toJsonPrettyStr(res);
            httpServletResponse.setContentType("application/json");
            out = httpServletResponse.getWriter();
            // 返回json信息给前端
            out.append(json);
            out.flush();
            return false;
        }

        if (username != null && password != null) {
            httpServletResponse.setCharacterEncoding("UTF-8");
            httpServletResponse.setContentType("application/json; charset=utf-8");
            PrintWriter out = null;
            try {
                //获取用户名和密码
                Props props = new Props("auth.properties");
                String userName = props.get(CommonConstant.USERNAME).toString();
                String pwd = props.get(CommonConstant.USERNAME).toString();
                if (userName.equals(userName) && pwd.equals(password)) {
                    return true;
                } else if (!userName.equals(userName) || !pwd.equals(password)) {
                    ResultBody res = new ResultBody();
                    res.setMsg("用户名或密码错误");
                    res.setCode("500");
                    String json = JSONUtil.toJsonPrettyStr(res);
                    httpServletResponse.setContentType("application/json");
                    out = httpServletResponse.getWriter();
                    // 返回json信息给前端
                    out.append(json);
                    out.flush();
                    return false;
                } else {
                    ResultBody res = new ResultBody();
                    res.setMsg("未登录，暂无权限");
                    res.setCode("500");
                    String json = JSONUtil.toJsonPrettyStr(res);
                    httpServletResponse.setContentType("application/json");
                    out = httpServletResponse.getWriter();
                    // 返回json信息给前端
                    out.append(json);
                    out.flush();
                    return false;
                }
            } catch (Exception e) {
                e.printStackTrace();
                httpServletResponse.sendError(500);
                return false;
            }

        }
        return true;
    }


    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
        // System.out.println("视图渲染之后的操作");
    }

}

```

## 三、总结
原来的冗余代码量至少三千行以上，之所以冗余客观历史背景是主要因素的。通过上面两种方式，有效地将代码量控制在一百行以内。虽然通过不同的方式进行重构某网API微服务的鉴权，但共同的目的则在于让程序更简化(化繁杂为简单、化多为少)。