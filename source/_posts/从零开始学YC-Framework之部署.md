---
title: 从零开始学YC-Framework之部署
date: 2022-11-11 16:36:48
tags: "YC-Framework"
---

不论是传统的开发模式还是互联网的开发模式，最终都是要部署上线的。
<!--more-->

针对YC-Framework目前的部署是执行jar包部署，**打包脚本内容如下:**
```
mvn clean package -Dmaven.test.skip=true

Cur_Dir=$(pwd)

TARGET_DIR="/D//test/jar//"

echo $Cur_Dir


if [ $? -ne 0 ]; then
        echo "fail"
else
    cp $Cur_Dir/yc-gateway/target/yc-gateway-1.0.jar $TARGET_DIR
	cp $Cur_Dir/yc-auth/target/yc-auth-1.0.jar $TARGET_DIR
	cp $Cur_Dir/yc-modules/yc-admin/target/yc-admin-1.0.jar $TARGET_DIR
    cp $Cur_Dir/yc-modules/yc-cms/target/yc-cms-1.0.jar $TARGET_DIR
    cp $Cur_Dir/yc-modules/yc-crawler/target/yc-crawler-1.0.jar $TARGET_DIR
    cp $Cur_Dir/yc-modules/yc-file/target/yc-file-1.0.jar $TARGET_DIR
    cp $Cur_Dir/yc-modules/yc-job/target/yc-job-1.0.jar $TARGET_DIR
    cp $Cur_Dir/yc-modules/yc-monitor-server/target/yc-monitor-server-1.0.jar $TARGET_DIR
    cp $Cur_Dir/yc-modules/yc-plugins/target/yc-plugins-1.0.jar $TARGET_DIR
	cp $Cur_Dir/yc-modules/yc-wechat/target/yc-wechat-1.0.jar $TARGET_DIR
    mvn clean
	echo "package success"
fi


```

打包成功以后，直接写一个scp命令上传对应的jar包到服务器，然后分别编写对应的服务部署脚本(通用模式):
```
nohup java -Xms512m -Xmx512m -jar xx-name.jar  > xx-name.log 2>&1 &

```

YC-Framework基于Spring生态中的SpringBoot。由于SpringBoot内嵌Tomcat，所以无需将其打成war包部署在Tomcat对应的webapps目录下。

除此之外YC-Framework不仅仅支持这样的部署，还支持docker、docker-compose、k8s的方式部署。

当然了，如果按照上述这样的部署方式，显然是不够灵活的，通常我们会将其步骤脚本化(或者使用jenkins)，从而达到自动化的目的。

目前不少企业推行落地DevOps来达到更加快速稳定地构建高质量的应用系统的目的。

记得看过一本书《分布式服务架构：原理、设计、实战》中提到DevOps，我觉得挺有参考意义的。

DevOps可以用一个公式表达：

**文化观念的改变+自动化工具=不断适应快速变化的市场。**

其核心价值在于两点：

- 1.**更快速地交付，响应市场变化**。
- 2.**更多地关注业务的改进与提升**。


**DevOps的开发流程可归纳如下:**

- 1.提交：工程师将程序在本地测试无问题后，提交到版本控制系统如Git或SVN等。
- 2.编译：持续整合系统（Jenkins）,在检测到版本更新时，便自动从Git/SVN仓库里拉取最新的程序，进行编译、构建。
- 3.单元测试：Jenkins完成编译构建后，会自动执行指定的单元测试代码。
- 4.部署到测试环境中：在完成单元测试后，Jenkins可将程序部署到与生产环境相近的测试环境中进行测试。
- 5.预生产环境：在预生产测试环境里，可以进行一些最后的自动化测试，例如Selenium测试及实际情况类似的测试，可由开发人员或客户手动进行。
- 6.部署到生产环境：通过所有测试后，便可将最新的版本部署到实际生产环境里。

从上述流程中可以看到，借助Jenkins等工具，提交程序之后的步骤几乎都可以自动化完成，这节省了工程师的大量手动操作时间。由于每次提交程序后都会自动进行编译与测试等流程，所以一旦有问题就可以马上发现并处理(Jenkins自动通知)，这样也保证了程序的质量。

**Jenkins文档::**
https://www.jenkins.io/doc/

**Docker文档:**
https://docs.docker.com/get-started/overview/

**K8S文档:**
https://kubernetes.io/docs/home/

**如何自建Git(可供参考五套方案):**

- (1)自建Gitlab。
- (2)自建Gitblit。
- (3)自建Gitea。
- (4)自建GitBucket。
- (5)自建Gogs。
- (6)自建Gitolite。

我本人选择Gitlab搭建，步骤如下:
```
//备份
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

//下载
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

//生成缓存
yum makecache

//安装
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
yum -y install gitlab-ce

//配置
cd /etc/gitlab/
vi gitlab.rb

external_url'http://gitlab.example.com' #域名或端口(如果是端口，需写为http://192.168.0.1:9090

//初始化
gitlab-ctl reconfigure

//启动
gitlab-ctl start

```

YC-Framework官网：
https://framework.youcongtech.com/

YC-Framework Github源代码：
https://github.com/developers-youcong/yc-framework

YC-Framework Gitee源代码：
https://gitee.com/developers-youcong/yc-framework

以上源代码均已开源，开源不易，如果对你有帮助，不妨给个star，鼓励一下！！！