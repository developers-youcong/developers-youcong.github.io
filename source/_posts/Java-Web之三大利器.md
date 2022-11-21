---
title: Java Web之三大利器
date: 2022-05-03 10:40:42
tags: "Java"
---

Java Web 三大利器主要有：

- 1.过滤器。
- 2.拦截器。
- 3.监听器。

<!--more-->

## 一、过滤器

### 1.什么是过滤器？
过滤器是JavaWeb的三大组件之一。

过滤器它是 JavaEE 的规范，可以在浏览器以及目标资源之间起到一个过滤的作用，它的作用是：拦截请求，过滤响应。

Web中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。

### 2.过滤器的应用场景有哪些？
- (1)登录验证。
- (2)权限检查。
- (3)事务管理。
- (4)统一编码处理。
- (5)敏感字符处理。

### 3.过滤器的生命周期是什么？
- (1)服务器启动，首先执行构造方法和init方法（这两个方法只执行一次）
- (2)当有匹配过滤条件的请求时执行doFilter方法（该方法可以执行多次）
- (3)服务器正常关闭的时候，或者该Filter类重新加载的时候会执行destroy方法（该方法只执行一次）

### 4.过滤器权限校验代码该怎么写？
```
@Component
public class FilterPerm implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init......");

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        System.out.println("FilterPerm," + request.getRequestURI());
        RespVO vo = new RespVO();
        PrintWriter out = null;
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json; charset=utf-8");
        try {
            String token = request.getParameter("token") == null ? null : request.getParameter("token");
            if (token == null || token == "") {
                vo.setCode(400);
                vo.setMsg("未携带Token");
            }
            if (token != null && "abcdef".equals(token)) {
                filterChain.doFilter(servletRequest, servletResponse);
                System.out.println("通过");
            }

            if (token != null && !"abcdef".equals(token)) {
                vo.setCode(403);
                vo.setMsg("访问未授权");
            }

        } catch (Exception e) {
            e.printStackTrace();

            vo.setCode(500);
            vo.setMsg("Server Error");
        }
        out = response.getWriter();
        String json = JSONUtil.toJsonPrettyStr(vo);
        // 返回json信息给前端
        out.append(json);
        out.flush();
    }

    @Override
    public void destroy() {
        System.out.println("destroy......");
    }
}

```

## 二、拦截器

### 1.什么是拦截器？
Java里的拦截器是动态拦截Action调用的对象，它提供了一种机制可以使开发者在一个Action执行的前后执行一段代码，也可以在一个Action执行前阻止其执行，同时也提供了一种可以提取Action中可重用部分代码的方式。

### 2.拦截器的应用场景有哪些？
- (1)权限控制。
- (2)日志打印。
- (3)参数校验。

### 3.拦截器如何实现对权限的控制？

核心代码：

**AuthInterceptor.java**
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

**WebConfig.java**
```
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private AuthInterceptor authInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(authInterceptor)
                .addPathPatterns("/**")//拦截所有的路径
                .excludePathPatterns(                         //添加不拦截路径
                        "/favicon.ico",
                        "/swagger-resources/**",              //js静态资源
                        "/webjars/**",             //css静态资源
                        "/api-doc/**",
                        "/doc.html",
                        "/v2/api-docs/**"
                );
    }
}


```

## 三、监听器

### 1.什么是监听器？
监听器，字面上的理解就是监听观察某个事件（程序）的发生情况，当被监听的事件真的发生了的时候，事件发生者（事件源） 就会给注册该事件的监听者（监听器）发送消息，告诉监听者某些信息，同时监听者也可以获得一份事件对象，根据这个对象可以获得相关属性和执行相关操作。

### 2.监听器的应用场景有哪些？
- (1)网站初始化。
- (2)统计在线人数。
- (3)统计网站访问量。
- (4)实现访问监控。

### 3.以统计在线人数，对应的监听器代码该如何编写？

**UserHttpSessionListener.java**
```

/**
 * 使用HttpSessionListener统计在线用户数的监听器
 */
@Component
public class UserHttpSessionListener implements HttpSessionListener {
 
    private static final Logger logger = LoggerFactory.getLogger(UserHttpSessionListener.class);
 
    /**
     * 记录在线的用户数量
     */
    public Integer count = 0;
 
    @Override
    public synchronized void sessionCreated(HttpSessionEvent httpSessionEvent) {
        logger.info("新用户上线了");
        count++;
        httpSessionEvent.getSession().getServletContext().setAttribute("count", count);
    }
 
    @Override
    public synchronized void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        logger.info("用户下线了");
        count--;
        httpSessionEvent.getSession().getServletContext().setAttribute("count", count);
    }
}

```


**TestController.java**
```
@RestController
@RequestMapping("/listener")
public class TestController {
 
   GetMapping("/total")
public String getTotalUser(HttpServletRequest request, HttpServletResponse response) {
    Cookie cookie;
    try {
        // 把sessionId记录在浏览器中
        cookie = new Cookie("JSESSIONID", URLEncoder.encode(request.getSession().getId(), "utf-8"));
        cookie.setPath("/");
        //设置cookie有效期为2天，设置长一点
        cookie.setMaxAge( 48*60 * 60);
        response.addCookie(cookie);
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }
    Integer count = (Integer) request.getSession().getServletContext().getAttribute("count");
    return "当前在线人数：" + count;
}

}


```