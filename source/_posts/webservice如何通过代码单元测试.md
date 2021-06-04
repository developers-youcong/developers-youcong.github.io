---
title: Webservice如何通过代码单元测试
date: 2020-11-21 11:07:59
tags: "SpringBoot"
---

前面我在下面这篇文章说过如何使用WebService:
[SpringBoot整合Apache-CXF实践](https://www.cnblogs.com/youcong/p/13874940.html)

在这篇文章中我列举过通过SOAP UI测试webservice接口。

但实际中涉及服务调用的情况，需要类似单元测试的东西。
<!--more-->

### 一、基于代理类工厂
核心代码很简单，如下所示(这个比较普遍常用):
```
 try {
            // 接口地址(写webservice的地址)
            String address = "http://127.0.0.1:9090/cxf/user?wsdl";
            // 代理工厂
            JaxWsProxyFactoryBean jaxWsProxyFactoryBean = new JaxWsProxyFactoryBean();
            // 设置代理地址
            jaxWsProxyFactoryBean.setAddress(address);
            // 设置接口类型
            jaxWsProxyFactoryBean.setServiceClass(UserService.class);
            // 创建一个代理接口实现
            UserService userService = (UserService) jaxWsProxyFactoryBean.create();

            return userService.addUser(email, username, password);
        } catch (Exception e) {
            e.printStackTrace();
            return -1;
        }

```

### 二、动态调用
```
        // 创建动态客户端
        JaxWsDynamicClientFactory dcf = JaxWsDynamicClientFactory.newInstance();
        //接口地址
        Client client = dcf.createClient("http://127.0.0.1/cxf/user?wsdl");
        // 需要密码的情况需要加上用户名和密码
        // client.getOutInterceptors().add(new ClientLoginInterceptor(USER_NAME, PASS_WORD));
        Object[] objects = new Object[0];
        try {
            // invoke("方法名",参数1,参数2,参数3....);
            objects = client.invoke("getUserName", "maple");
            System.out.println("返回数据:" + objects[0]);
        } catch (java.lang.Exception e) {
            e.printStackTrace();
        }

```

### 三、通过Controller调用
我在[SpringBoot整合Apache-CXF实践](https://www.cnblogs.com/youcong/p/13874940.html)
这篇文章里就写到过。

具体的代码例子如下:
```
package com.blog.cxf.client.controller;

import com.blog.cxf.server.dto.UserReqDto;
import com.blog.cxf.server.service.UserService;
import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;
import org.springframework.web.bind.annotation.*;

/**
 * @description:
 * @author: youcong
 * @time: 2020/10/24 23:37
 */
@RestController
@RequestMapping("/user")
public class UserApiController {


    @PostMapping("/add")
    public int add(@RequestParam String email, @RequestParam String username, @RequestParam String password) {

        try {
            // 接口地址
            String address = "http://127.0.0.1:9090/cxf/user?wsdl";
            // 代理工厂
            JaxWsProxyFactoryBean jaxWsProxyFactoryBean = new JaxWsProxyFactoryBean();
            // 设置代理地址
            jaxWsProxyFactoryBean.setAddress(address);
            // 设置接口类型
            jaxWsProxyFactoryBean.setServiceClass(UserService.class);
            // 创建一个代理接口实现
            UserService userService = (UserService) jaxWsProxyFactoryBean.create();

            return userService.addUser(email, username, password);
        } catch (Exception e) {
            e.printStackTrace();
            return -1;
        }
    }
}

```

至于其它服务调用，可以通过Feign，也可以基于HtppClient。


至于上面三种哪种更好，具体看实际业务场景而定。可以根据自己的需求来选择。

最后向大家推荐一下，近来所写关于WebService安全机制的思考和实践一文:
[WebService安全机制的思考与实践](https://www.cnblogs.com/youcong/p/13906999.html)