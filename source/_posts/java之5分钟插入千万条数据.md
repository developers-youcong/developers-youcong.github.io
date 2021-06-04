---
title: java之5分钟插入千万条数据
date: 2020-09-09 21:00:11
tags: "Java"
---
虽说不一定5分钟就插入完毕，因为取决去所插入的字段，如果字段过多会稍微慢点，但不至于太慢。10分钟内基本能看到结果。

之前我尝试用多线程来实现数据插入(百万条数据)，半个多小时才二十多万条数据。

线程池数据插入核心代码:
```
ExecutorService executorService = Executors.newFixedThreadPool(1000000);
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 150000; i++) {
                    TestUser user = new TestUser();
                    user.setName(RandomUtil.randomString(20));
                    userDao.insert(user);
                    System.out.println("插入数据:" + i);
                }

                System.out.println(Thread.currentThread().getName() + "正在执行任务");

            }
        });

```


应用场景:
造测试数据，如千万甚至亿万级别的数据自动快速生成。
<!--more-->
关键核心实现类代码如下:
```

        long startTime = System.currentTimeMillis();
        try {

            for (int i = 0; i < 10000; i++) {
                List<TestUser> users = new ArrayList<>();

                for (int j = 0; j < 1000; j++) {
                    TestUser user = new TestUser();
                    user.setName(RandomUtil.randomString(20));
                    user.setName2(RandomUtil.randomString(20));
                    user.setName3(RandomUtil.randomString(20));
                    user.setName4(RandomUtil.randomString(20));
                    user.setName5(RandomUtil.randomString(20));
                    user.setName6(RandomUtil.randomString(20));
                    user.setName7(RandomUtil.randomString(20));
                    user.setName8(RandomUtil.randomString(20));
                    user.setName10(RandomUtil.randomString(20));
                    user.setName11(RandomUtil.randomString(20));
                    user.setName12(RandomUtil.randomString(20));
                    user.setName13(RandomUtil.randomString(20));
                    user.setName14(RandomUtil.randomString(20));
                    user.setName15(RandomUtil.randomString(20));
                    user.setName16(RandomUtil.randomString(20));
                    user.setName17(RandomUtil.randomString(20));
                    user.setName18(RandomUtil.randomString(20));
                    user.setName19(RandomUtil.randomString(20));
                    user.setName20(RandomUtil.randomString(20));

                    users.add(user);
                }

                int changed = userDao.batchAdd(users);

                System.out.println("#" + i + " changed=" + changed);

            }
        } catch (Exception ex) {
            ex.printStackTrace();
        } finally {
            long endTime = System.currentTimeMillis();
            System.out.println("Time elapsed:" + toDhmsStyle((endTime - startTime) / 1000) + ".");
        }

```
代码原理:
插入一千条数据后提交一次，然后重复一万次的方式。

关键核心DAO:
```
@Repository
public interface TestUserDao extends BaseMapper<TestUser> {

    int batchAdd(@Param("users") List<TestUser> users);
}


```
XML:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.springcloud.blog.practice.dao.TestUserDao">

    <insert id="batchAdd">
        insert into test_user(name,name2,name3,name4,name5,name6,name7,name8,name9,name10,name11,name12,name13,name14,name15,name16,name17,name18,name19,name20)
        values
        <foreach collection="users" item="item" separator=",">
            (#{item.name},#{item.name2},#{item.name3},#{item.name4},#{item.name5},#{item.name6},#{item.name7},#{item.name8},#{item.name9},#{item.name10},#{item.name11},#{item.name12},#{item.name13},#{item.name14},#{item.name15},#{item.name16},#{item.name17},#{item.name18},#{item.name19},#{item.name20})
        </foreach>
    </insert>
</mapper>


```

参考链接:
[[MyBatis]五分钟向MySql数据库插入一千万条数据 批量插入 用时5分左右](https://www.cnblogs.com/heyang78/p/11661796.html)