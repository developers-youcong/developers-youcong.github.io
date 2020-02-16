---
title: nexus没有授权导致的错误
date: 2019-10-26 20:52:23
tags: "maven"
---

错误详情信息:
```
错误信息:
[ERROR] Failed to execute goal on project blog: Could not resolve dependencies for project com.youcong.test:blog:war:1.0.0: Failed to collect dependencies at com.youcong.test:blog-platform-lib:jar:2.3.06: Failed to read artifact descriptor for com.youcong.test:blog-platform-lib:jar:2.3.06: Could not transfer artifact com.youcong.test:blog-platform-lib:pom:2.3.06_190620 from/to nexus (私服URL): Not authorized -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException

```
<!--more-->

执行mvn install 导致的错误，原因是因为nexus没有授权。

解决方案:
nexus授权即可(在maven对应的settings.xml配置私服即可)

