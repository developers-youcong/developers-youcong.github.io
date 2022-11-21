---
title: 我在M2公司做架构之Nexus私服
date: 2021-10-30 15:20:53
tags: ["架构","职业生涯","技术思考与实践"]
---

## 一、Nexus介绍
Nexus是一个强大的Maven仓库管理器,它极大的简化了本地内部仓库的维护和外部仓库的访问。
<!--more-->

Nexus是一套开箱即用的系统不需要数据库,它使用文件系统加Lucene来组织数据。

Nexus使用ExtJS来开发界面,利用Restlet来提供完整的REST APIs,通过IDEA和Eclipse集成使用。

Nexus支持WebDAV与LDAP安全身份认证。

Nexus提供了强大的仓库管理功能,构件搜索功能,它基于REST,友好的UI是一个extjs的REST客户端,占用较少的内存,基于简单文件系统而非数据库。

## 二、什么是私服？
私服是指私有服务器,是假设在局域网的一种特殊的远程仓库,目的是代理远程仓库及部署第三方构建.有了私服之后,当Maven需要下载构件时,直接请求私服,私服上存在则下载到本地仓库;否则,私服请求外部的远程仓库,将构件下载到私服,在提供给本地仓库下载.


## 二、为什么要使用私服？私服的好处有哪些？
如果没有Nexus私服，我们所需的所有构件都需要通过Maven的中央仓库和第三方的Maven仓库下载到本地，而一个团队中的所有人都重复的从Maven仓库下载构件无疑加大了仓库的负载和浪费了外网带宽，如果网速慢的话，还会影响项目的进程。很多情况下项目的开发都是在内网进行的，连接不到Maven仓库怎么办呢？开发的公共构件怎么让其它项目使用？这个时候我们不得不为自己的团队搭建属于自己的Maven私服，这样既节省了网络带宽也会加速项目搭建的进程，当然前提条件就是你的私服中拥有项目所需的所有构件。

私服的好处由此归纳如下:

- 节省外网带宽；
- 加速Maven构建；
- 部署第三方构件；
- 提高稳定性，增强控制；
- 降低中央仓库的负荷；
- 控制和审计；
- 建立本地内部公用仓库。

## 三、Nexus搭建步骤
以前在创业公司的时候搭建过私服，文章如下:
[Maven搭建私服](https://www.cnblogs.com/youcong/p/9416938.html)

[Nexus修改admin密码及其添加用户](https://www.cnblogs.com/youcong/p/9438956.html)

[nexus没有授权导致的错误](https://www.cnblogs.com/youcong/p/11745609.html)

上面涉及的比较偏老版本，去年为公司搭建了一个新的Nexus私服，版本相对比较新，搭建的流程总结了一番。

### 1.官网下载
https://help.sonatype.com/repomanager3/download/download-archives---repository-manager-3

或者使用wget下载
```
wget https://download.sonatype.com/nexus/3/nexus-3.29.2-02-unix.tar.gz

```

wget下载不了，可能需要VPN。

### 2.解压
```
tar -xzvf nexus-3.29.2-02-unix.tar.gz

```
### 3.配置运行用户
```
vim nexus.rc
#取消改行注释并改为如下:
run_as_user="user"

```

### 4.修改端口
```
vim nexus-default.properties
将端口改为如下:
application-port=9951

```

### 5.启动
```
./nexus start

```

### 6.maven settings.xml配置
```

    <server>
        <id>maven-releases</id>
        <username>user</username>
        <password>xxxx</password>
    </server>
	
    <server>
        <id>maven-snapshots</id>
        <username>user</username>
        <password>xxxx</password>
    </server>
	
   <mirror>
        <!--该镜像的唯一标识符。id用来区分不同的mirror元素。 -->
        <id>maven-public</id>
        <!--镜像名称 -->
        <name>maven-public</name>
        <!--*指的是访问任何仓库都使用我们的私服-->
        <mirrorOf>*</mirrorOf>
        <!--该镜像的URL。构建系统会优先考虑使用该URL，而非使用默认的服务器URL。 -->
        <url>http://127.0.0.1:9951/repository/maven-public/</url>     
    </mirror>

```

### 7.项目pom.xml配置
```
<distributionManagement>
        <repository>
            <id>maven-releases</id>
            <name>maven-releases</name>
            <url>http://127.0.0.1:9951/repository/maven-releases/</url>
        </repository>
    </distributionManagement>

或
    <repositories>
        <repository>
            <id>maven-public</id>
            <name>maven-public</name>
            <url>http://127.0.0.1:9951/repository/maven-public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>

```

### 8.Nexus常用命令
```
./nexus start
./nexus status
./nexus stop

```

## 四、我推行私服主要解决的是哪些问题？
**主要解决的问题是:**让通用型组件在不同的微服务中以Maven依赖的形式进行注入，而通用型组件由专门的团队或者是专门人来进行维护，一定程度上让通用型组件与微服务这些关系进行隔绝，避免因为同在一个项目之中，有的地方一改动，另外的地方也随之改动，从而导致了牵其一动其余。而采用了私服，每次通用型组件发布都会有对应的版本号，小版本小改动，大版本大改动，不影响原有的微服务团队使用当前版本的通用型组件，通用型组件的发布不影响现有微服务的通用型组件使用，至于最终是否升级，依据各微服务团队而定，结合具体情况，安排时间，逐步迭代。终极目标始终围绕了软件的设计原则:高内聚，低耦合。