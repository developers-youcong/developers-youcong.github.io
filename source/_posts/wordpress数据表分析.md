---
title: wordpress数据表分析
date: 2019-02-23 13:19:24
tags: "wordpress"
---
wordpress一共是有12个表的:
|wp_commentmeta        |
| wp_comments           |
| wp_links              |
| wp_options            |
| wp_postmeta           |
| wp_posts              |
| wp_term_relationships |
| wp_term_taxonomy      |
| wp_termmeta           |
| wp_terms              |
| wp_usermeta           |
| wp_users         		|


主要参考这篇文章:[WordPress数据库及各表结构分析](https://www.cnblogs.com/wordblog/p/6591499.html)
另外没有搭建过wordpress的朋友们，可以参考我的这篇文章搭建wordpress和对wordpress有一个大致的了解，文章为[wordpress研究](https://www.cnblogs.com/youcong/p/9671294.html)
<!--more-->
十二个表对应的职能如下所示:

wp_commentmeta:存储评论的元数据
wp_comments:存储评论
wp_links:存储友情链接
wp_options:存储wordpress系统选项和插件、主题配置
wp_postmeta:存储文章(包括页面、上传文件、修订)的元数据
wp_posts:存储文章(包括页面、上传文件、修订)
wp_term_relationships:存储每个文章、链接和对应分类的关系
wp_term_taxonomy:存储每个目录、标签所对应的分类
wp_termmeta:存储网站分类和标签的属性
wp_terms:存储每个目录、标签
wp_usermeta:存储用户的元数据
wp_users:存储用户


#### wp_commentmeta:

meta_id:自增唯一ID
comment_id:对应评论ID
meta_key:键名
meta_value:键值


#### wp_comments
comment_ID:自增唯一ID
comment_post_ID:对应文章ID
comment_author:评论者
comment_author_email:评论者邮箱
comment_author_url:评论者网址
comment_date:评论时间
comment_date_gmt:评论时间(GMT+0时间)
comment_content:评论正文
comment_karma:未知(通过搜索引擎查找，这个字段在wordpress中并没有起到作用)
comment_approved:评论是否被批准
comment_agent:评论者的USER_AGENT
comment_type:评论类型([PingBack](https://baike.baidu.com/item/PingBack/6316909?fr=aladdin)/普通)
comment_parent:父评论ID
user_id:评论者用户ID(不一定存在，考虑到游客或者其它因素)


#### wp_links
link_id:自增唯一ID
link_url:链接URL
link_name:链接标题
link_image:链接图片
link_target:链接打开方式
link_description:链接描述
link_visible:是否可见(Y/N)
link_owner:添加者用户ID
link_rating:评分等级
link_updated:未知
link_rel:XFN关系(关于XFN关系，了解详情，请参考该篇博文:https://www.fujieace.com/wordpress/xfn.html)
link_notes:XFN注释
link_rss:链接RSS地址


#### wp_options
option_id:自增唯一ID
blog_id:博客ID，用于多用户博客，默认为0
option_name:键名
option_value:键值
authload:在WordPress载入时自动载入(yes/no)


#### wp_postmeta
meta_id:自增唯一ID
post_id:对应文章ID
meta_key:键名
meta_value:键值

#### wp_posts
ID:自增唯一ID
post_author:对应作者ID
post_date:发布时间
post_date_gmt:发布时间(GMT+0时间)
post_content:正文
post_title:标题
post_excerpt:摘录
post_status:文章状态(publish/auto-draft/inherit)
post_password:文章密码
post_name:文章缩略名
to_ping:ping的链接
pinged:已经PING过的链接
post_modified:修改时间
post_modified_gmt:修改时间(GMT+0时间)
post_content_filtered:未知
post_parent:父文章，主要用于page
guid:未知
menu_order:排序ID
post_type:文章类型(post/page等)
post_mime_type:MIME类型
comment_count:评论总数

#### wp_terms
term_id:分类ID
name:分类名
slug:缩略名
term_group:未知

#### wp_term_relationships
object_id:对应文章ID/链接ID
term_taxonomy_id:对应分类方法ID
term_order:排序


#### wp_term_taxonomy
term_taxonomy_id:wp_term_taxonomy表ID
term_id:对应wp_terms表中的ID
taxonomy:表示分类系统(category/post_tag)
description:分类描述
parent:父分类ID
count:分类文章总数

#### wp_usermeta
umeta_id:自增唯一ID
user_id:对应用户ID
meta_key:键名
meta_value:键值

#### wp_users
ID:自增唯一ID
user_login:登录名
user_pass:密码
user_nickname:昵称
user_email:邮箱
user_url:网址
user_registered:注册时间
user_status:用户状态
display_name:显示名称


通过上述我们知道了wordpress十二张表的含义了。但是我仍然不打算快速入手开发。
原因很简单，感性认识不够。
那么如何加深这个感性认识呢？
那就是使用。
我将把我在上面的使用写一个文档。

wordpress针对用户有这么几个角色设置(对应着权限):
管理员、订阅者、投稿者、作者、编辑等。

以我个人的理解如下:

管理员肯定是拥有绝对权限的。

订阅者，就好比我们订报纸，每天早上都会有邮政的人将报纸送到邮箱里，我们就可以拿起报纸阅读了。

投稿者:投稿者就更好理解了，写完稿子递交上去，如果稿子ok没有问题，就可以在对应的周刊上登记了。

作者:以我在博客园发布文章，博客园作为一个平台，我在上面可以随时编写文章然后发布，不需要经过任何人审批以后才能发布(当然了，发布首页给广大的朋友们看，还是需要经过审批的)

编辑:我觉得可以和阿里云云栖社区联系起来，我之前将博客园迁移到云栖社区，在该社区每次发布一篇文章需要经过人工审核，人工审核通过后才能给别人看到，这个编辑可以随意删除文章禁止文章发布。

关于wordpress权限含义可以参考这篇文章:https://baijiahao.baidu.com/s?id=1611569585137454290&wfr=spider&for=pc




