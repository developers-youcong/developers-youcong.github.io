---
title: 为什么bootstrap.yml比application.yml优先级高
date: 2020-11-05 20:07:42
tags: "SpringBoot"
---

最近遇到好几个与此有关的问题。

其中一个典型的问题是，明明bootstrap.yml指定了端口，但还是显示为默认的8080端口。最后我用了一个常规的死办法将bootstrap.yml改为application.yml就好了。

于是我不得不思考，为什么会出现这样的问题。通过搜索我了解到了以下几点。
<!--more-->
### 为什么bootstrap.yml会比application.yml先加载？
启动上下文时，Spring Cloud 会创建一个 Bootstrap Context，作为 Spring 应用的 Application Context 的父上下文。

初始化的时候，Bootstrap Context 负责从外部源加载配置属性并解析配置。这两个上下文共享一个从外部获取的 Environment。Bootstrap 属性有高优先级，默认情况下，它们不会被本地配置覆盖。

也就是说如果加载的 application.yml 的内容标签与 bootstrap 的标签一致，application 也不会覆盖 bootstrap，而 application.yml 里面的内容可以动态替换。



