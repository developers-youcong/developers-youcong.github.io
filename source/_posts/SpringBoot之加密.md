---

title: SpringBoot之加密

date: 2019-02-22 20:26:44

tags: "SpringBoot"

---



最近利用闲暇时间写了一个博客系统，主要参考wordpress，主要目的是为了提高自己的技术能力。

写代码写了两年多，联系到之前在学校的时候写过的一个博客系统，发现工作中开发的系统，技术上基本一致，业务逻辑方面存在差异。

比如博客系统可能面对高并发的场景，比如某个时间段访问量，再比如博客系统为了最大程度吸引用户(换句话说，提高用户粘性)，在界面上美观，使用上更加方便。通常界面美观伴随着前端js库的增多，特别是一些非常好看的画面或者图像是极其消耗带宽的，带宽如果不给力的话，页面半天打不开，同样也对于用户而言体验不好。



只想表达一个意思，软件开发万变不离其宗，本质上基本都是一个CMS系统(也许这句话武断了)。



话不多说，今天我要讲的关于SpringBoot加密。

<!--more-->

SSM框架和SpringBoot中加密是不一样的，比如在SSM框架中可以使用Druid进行加密(主要对数据库密码或者是其它重要配置信息加密)，但是在SpringBoot就不一定会适用。



关于Druid加密，可以参考我的博客园这篇文章:[Druid加密](https://www.cnblogs.com/youcong/p/10140043.html)



SpringBoot使用jasypt-spring-boot-starter加密，具体步骤如下:



## 一、导入Maven依赖(注意，我的springboot版本为1.5.9，建议最好版本别相差太多，否则会出现依赖冲突等问题)

```

	<dependency>

			<groupId>com.github.ulisesbocchio</groupId>

			<artifactId>jasypt-spring-boot-starter</artifactId>

			<version>1.16</version>

	</dependency>

```





## 二、编写测试类

```

package cn.test;



import org.jasypt.encryption.pbe.StandardPBEStringEncryptor;

import org.jasypt.encryption.pbe.config.EnvironmentPBEConfig;

import org.junit.Test;

import org.junit.runner.RunWith;

import org.springframework.beans.factory.annotation.Autowired;

import org.springframework.boot.test.context.SpringBootTest;

import org.springframework.data.redis.core.StringRedisTemplate;

import org.springframework.test.context.junit4.SpringRunner;



import com.blog.springboot.Application;

import com.blog.springboot.service.UsersService;



import cn.hutool.core.util.RandomUtil;

@RunWith(SpringRunner.class)

@SpringBootTest(classes = Application.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)

public class JunitTest {





	@Test

	public void testEncrypt() throws Exception {

	        StandardPBEStringEncryptor standardPBEStringEncryptor = new StandardPBEStringEncryptor();

	        EnvironmentPBEConfig config = new EnvironmentPBEConfig();

	        config.setAlgorithm("PBEWithMD5AndDES");          // 加密的算法，这个算法是默认的

	        config.setPassword("lyh");                        // 加密的密钥

	        standardPBEStringEncryptor.setConfig(config);

	        //加密用户信息

	        String plainText = "youcong";

	        String encryptedText = standardPBEStringEncryptor.encrypt(plainText);

	        //加密密码信息

	        String Enpassword = "youcong";

	        String EnpasswordText = standardPBEStringEncryptor.encrypt(Enpassword);

	        String db="wordpress";

	        String dbEnc = standardPBEStringEncryptor.encrypt(db);

	        //加密地址信息

	        String DBAUrl = "jdbc:mysql://localhost:3306/blog?autoReconnect=true&useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=false";

	        String DBAUrlText = standardPBEStringEncryptor.encrypt(DBAUrl);

	        System.out.println("用户:"+encryptedText);

	        System.out.println("密码:"+EnpasswordText);

	        System.out.println("地址:"+DBAUrlText);

	        System.out.println("db:"+dbEnc);

	    }



}



```



## 三、在springboot的配置文件添加如下配置(这里我以application.yml配置为例)

```

jasypt:

  encryptor:

    password: lyh

```



问:为什么要加这段？

答:这里的password对应的值lyh相当于密钥，主要用于解密。

你在单元测试中以什么作为加密，那么在yml中就以什么作为解密。





## 四、配置application.yml中的数据源

```

  datasource:

    url: ENC(cY3NmQF349TpBB0z0KavaiEPNDux/mKEss0UFeA11VTFC545rHh6t1rLC46GlX1b2rm8s5lzX49JmzFE4odcSiPafGZfQvnsHl2yVlLWM3kJg5DvVI4D0l5na3RUPTio4uz1gG9nML1u9ceHuj/yPb1097ZZfbCUsLSyRoeWvhhKuPxAM5mvGLZh641ArtVfRchNcdVZ1W4=)

    username: ENC(BcbIdbvEq4yN8kezH5mDjg==)

    password: ENC(Isk3pYM71258wxWTQOt3Dg==)

    db-name: ENC(CZcfw3ZJN6TVCVxkCW9Ey6z6iAuszHO8)

    filters: log4j,wall,mergeStat1

```



问:为什么要在数据源中添加ENC?

答:这个ENC相当于解密的标识符。



