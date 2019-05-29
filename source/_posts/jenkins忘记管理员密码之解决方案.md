---
title: jenkins忘记管理员密码之解决方案
date: 2019-03-07 19:34:40
tags: "jenkins"
---

jenkins忘记管理员密码怎么办？

通常有这么几种解决方案，如下所示:

<!--more-->

(1)进入对应的用户目录文件夹，以ubuntu16.04为例，jenkins安装目录为/var/lib/jenkins
进入到该目录，通过ls或ll命令可以显示对应的所有文件夹，找到其中的users文件夹，进入对应的用户里面，修改config.xml中的passwordHash
通过关键字hudson.security.HudsonPrivateSecurityRealm_-Details即可找到。如果找不到你可以采用方案(2)解决该问题;

(2)还是以ubuntu为了，进入/var/lib/jenkins目录，修改config.xml中的useSecurity将其改为false
```
  <mode>NORMAL</mode>
  <useSecurity>false</useSecurity> //默认为true
  <authorizationStrategy class="hudson.security.AuthorizationStrategy$Unsecured"/>
```

最后通过service jenkins restart重启jenkins，请按照如下步骤依次操作:
依次点击页面中的系统管理->全局安全配置->勾选启用安全->选择jenkins专有用户数据库，点击保存->再次点击系统管理->管理用户，配置管理员账号密码即可

就这样愉快地解决了问题。

参考链接如下所示:
Jenkins忘记密码解决方案:https://www.cnblogs.com/kazihuo/p/9328107.html
Jenkins 忘记admin密码拯救方法:https://www.cnblogs.com/huangenai/p/7416322.html