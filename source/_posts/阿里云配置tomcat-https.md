---
title: 阿里云配置tomcat https
date: 2019-02-25 19:22:15
tags: "Linux"
---
阿里云申请免费的SSL证书和配置https，可参考该篇博文:https://blog.csdn.net/baidu_19473529/article/details/70037976

虽然有现成的，不过我还是要做一个小小的总结记录一下。

由于我公司使用的tomcat主要是8.5,所以我以8.5配置作为讲解(总的来说，配置相差不大)

假定你已经在阿里云成功申请到免费的SSL证书，现在我们开始来配置。
<!--more-->
## 1.首先讲pfx文件放在tomcat的一个叫cert的文件夹(tomcat实际上没有这个文件夹，你可以选择手动创建一个)
```
cd tomcat8
mkdir cert
```

## 2.修改server.xml
```
   <Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true"  scheme="https"
    secure="true"
               >
        <SSLHostConfig>
             <Certificate certificateKeystoreFile="cert/test.pfx"
                         certificateKeystoreType="PKCS12" certificateKeystorePassword="1541231341210" />
        </SSLHostConfig>
    </Connector>


    <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="443" />

    <Connector port="8009" protocol="AJP/1.3" redirectPort="443" />


```

## 3.修改web.xml
编辑web.xml，不加下面这段的话不能把http请求转为https
在该文件</welcome-file-list>标签（一般在文件最末尾）后面加上这样一段：
```
<login-config>  
    <!-- Authorization setting for SSL -->  
    <auth-method>CLIENT-CERT</auth-method>  
    <realm-name>Client Cert Users-only Area</realm-name>  
</login-config>  
<security-constraint>  
    <!-- Authorization setting for SSL -->  
    <web-resource-collection >  
        <web-resource-name >SSL</web-resource-name>  
        <url-pattern>/*</url-pattern>  
    </web-resource-collection>  
    <user-data-constraint>  
        <transport-guarantee>CONFIDENTIAL</transport-guarantee>  
    </user-data-constraint>  
</security-constraint>

```

本文主要参考该篇博文[Tomcat8、9配置https SSL证书 阿里云的免费dv证书](https://blog.csdn.net/qq_35624642/article/details/83016813)

另外为了调试方便，本地tomcat还需配置https,关于windowns配置https，可参考该篇博文[本地windows搭建https](https://blog.csdn.net/u014793522/article/details/54846973)