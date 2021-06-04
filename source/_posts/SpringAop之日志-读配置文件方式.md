---
title: SpringAop之日志(读配置文件方式)
date: 2020-08-23 11:44:51
tags: "SpringBoot"
---

读配置文件的目的在于减少代码上的冗余，这个冗余通常指加注解之类的。
<!--more-->

比方说，我们原来的代码是这样:
```
    @GetMapping("/list")
    @Log(title = "查询用户列表", businessType = BusinessType.QUERY)
    public AjaxResult list() {

        return AjaxResult.success(userService.queryUserListInfo());
    }

```

相当于每个我都要加上@Log，我才能在aop中将其插入日志表(识别功能)，现在我觉得这样太麻烦了。所以我可以这样做，也就是将@Log抽取一个配置文件，这个配置文件，团队定个规矩，上线前统一更改即可。

于是我的AOP代码就变成这样:
```
@Aspect
@Component
public class LogAspect {


    public static LogAspect logAspect;

    @PostConstruct
    public void init() {
        logAspect = this;
    }

    @Pointcut("execution(public * com.eqics.blog.controller..*.*(..))")
    public void Pointcut() {

        System.out.println("切点");
    }


    //@Around：环绕通知
    @Around("Pointcut()")
    @Transactional(isolation = Isolation.DEFAULT)
    public Object Around(ProceedingJoinPoint pjp) throws Throwable {
        Map<String, Object> data = new HashMap<>();
        //获取目标类名称
        String clazzName = pjp.getTarget().getClass().getName();
        //获取目标类方法名称
        String methodName = pjp.getSignature().getName();

        // 请求的地址
        String ip = IpUtils.getIpAddr(ServletUtils.getRequest());

        String apiUrl = ServletUtils.getRequest().getRequestURI();

         //关键核心代码
        InputStream path = getClass().getResourceAsStream("/api.properties");
        BufferedReader reader = new BufferedReader(new InputStreamReader(path));
        System.out.println("reader:" + reader);


        Properties pro = new Properties();
        pro.load(reader);

        System.out.println("pro:" + pro.getProperty(apiUrl));

        data.put("apiUrl", apiUrl);

        //IP地址
        data.put("ip", ip);

        //记录类名称
        data.put("clazzName", clazzName);
        //记录对应方法名称
        data.put("methodName", methodName);
        //记录请求参数
        data.put("params", pjp.getArgs());

        //开始调用时间
        // 计时并调用目标函数
        long start = System.currentTimeMillis();
        Object result = pjp.proceed();
        Long time = System.currentTimeMillis() - start;

        //记录返回参数
        data.put("result", result);

        //设置消耗总时间
        data.put("consumeTime", time);

        try {

            System.out.println("日志输出:" + data);

        } catch (Exception e) {
        
            e.printStackTrace();
        }

        return result;

    }


}

```

api.properties配置文件如下:
```
# 用户管理
/blog_user/list=get user manage
/blog_user/list_test=用户管理测试


```