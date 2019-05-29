---
title: >-
  mybatis错误之org.apache.ibatis.binding.BindingException: Invalid bound statement
  (not found)
date: 2019-03-05 21:16:45
tags: "MyBatis"
---
玩了MyBatis差不多有两年了，中间也玩过MyBatis-Plus,这个MyBatis-Plus其实与MyBatis的区别并不大。今天写博客业务代码的时候，犯一个初学者犯过的错误。
<!--more-->
错误信息如下:
org.apache.ibatis.binding.BindingException: Invalid bound statement
  (not found)


通常原因是因为Mapper interface和xml文件的定义对不上，通常需要检查包名、namespace、函数名等。

出现这个错误的原因是我太过相信自我了，觉得自觉没有错，于是手打，结果就是一个单词写错了。

看代码示例:

xml:
```
   <select id="resentPosts" resultMap="BaseResultMap">
    	SELECT post_title FROM `wp_posts` WHERE post_status = 'publish' ORDER BY post_modified DESC LIMIT 0,5 
    </select>
```

dao(interface):
```
	//近期文章
	public List<Posts> recentPosts();
```




大家很容易会看出select标签中的id与dao中的接口函数名不对应。这就是问题的根源，改成一样的，如下(即可解决问题)

```
  <select id="recentPosts" resultMap="BaseResultMap">
    	SELECT post_title FROM `wp_posts` WHERE post_status = 'publish' ORDER BY post_modified DESC LIMIT 0,5 
    </select>
```

最后说一句，遇到问题不要慌，找到问题关键信息，复制到百度上/谷歌或者stackoverflow即可找到答案。

太阳底下没有新鲜事儿，你遇到过的，说不定别人也遇到过。

参考链接:https://www.cnblogs.com/lfm601508022/p/InvalidBoundStatement.html