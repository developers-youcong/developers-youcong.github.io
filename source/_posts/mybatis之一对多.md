---
title: mybatis之一对多
date: 2019-03-06 20:54:56
tags: "MyBatis"
---
今天主要话题围绕这么几个方面？
- mybatis一对多示例
- sql优化策略
<!--more-->
## 一、mybatis之一对多
在说一对多之前，顺便说一下一对一。

一对一，常见的例子，比如以常见的班级例子来说，一个班主任只属于一个班级(排除某个班主任能力超群可兼任多个班级).

例如:
```
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE mapper    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"    
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<!--  为这个mapper指定一个唯一的namespace，namespace的值习惯上设置成包名+sql映射文件名，这样保证了namespace的值是唯一的-->
<mapper namespace="com.yc.mybatis.test.classMapper">
    
        <!-- 
         方式一：嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集
                 封装联表查询的数据(去除重复的数据)
         select * from class c, teacher t where c.teacher_id=t.t_id and c.c_id=1
     -->
    
    <select id="getClass" parameterType="int" resultMap="getClassMap">
        select * from class c, teacher t  where c.teacher_id = t.t_id and c.teacher_id=#{id}
    </select>
    
    <!-- resultMap:映射实体类和字段之间的一一对应的关系 -->
    <resultMap type="Classes" id="getClassMap">
        <id property="id" column="c_id"/>   
        <result property="name" column="c_name"/>
        <association property="teacher" javaType="Teacher">   
            <id property="id" column="t_id"/>
            <result property="name" column="t_name"/>
        </association>
    </resultMap>
    
     <!-- 
         方式二：嵌套查询：通过执行另外一个SQL映射语句来返回预期的复杂类型
         SELECT * FROM class WHERE c_id=1;
         SELECT * FROM teacher WHERE t_id=1   //1 是上一个查询得到的teacher_id的值
         property:别名(属性名)    column：列名 -->
          <!-- 把teacher的字段设置进去 -->
    <select id="getClass1" parameterType="int" resultMap="getClassMap1">
        select * from class where c_id=#{id}
    </select>
    
    <resultMap type="Classes" id="getClassMap1">
        <id property="id" column="c_id"/>   
        <result property="name" column="c_name"/>
        <association property="teacher" column="teacher_id" select="getTeacher"/>   
    </resultMap>
    <select id="getTeacher" parameterType="int" resultType="Teacher">
        select t_id id,t_name name from teacher where t_id =#{id}
    </select>
</mapper>
```

顺便对association标签的属性进行解释:
property:对象属性名称
javaType:对象属性类型
column:所对应的外键字段名称

一对多，以我博客为例，比如今天我写的一个近期评论的接口就是一个一对多的体现(一个评论者可以对应多篇文章，相反，多篇文章也能对应一个评论者，从中可以体现一对多，多对一，甚至多对多的关系)

关于一对一、一对多或者多对多，可以参考[Mybatis 一对一，一对多，多对一，多对多的理解](https://www.cnblogs.com/yaobolove/p/5444046.html)

话不多说，看xml代码:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.blog.springboot.dao.CommentsDao">

    <!-- 通用查询映射结果 -->
    <resultMap id="BaseResultMap" type="com.blog.springboot.entity.Comments">
    
        <id column="comment_ID" property="commentId" />
        <result column="comment_post_ID" property="commentPostId" />
        <result column="comment_author" property="commentAuthor" />
        <result column="comment_author_email" property="commentAuthorEmail" />
        <result column="comment_author_url" property="commentAuthorUrl" />
        <result column="comment_author_IP" property="commentAuthorIp" />
        <result column="comment_date" property="commentDate" />
        <result column="comment_date_gmt" property="commentDateGmt" />
        <result column="comment_content" property="commentContent" />
        <result column="comment_karma" property="commentKarma" />
        <result column="comment_approved" property="commentApproved" />
        <result column="comment_agent" property="commentAgent" />
        <result column="comment_type" property="commentType" />
        <result column="comment_parent" property="commentParent" />
        <result column="user_id" property="userId" />
        
        <collection property="posts" ofType="Posts">
        	<result column="post_title" property="postTitle"/>
        </collection>
        
    </resultMap>

    <!-- 通用查询结果列 -->
    <sql id="Base_Column_List">
        comment_ID AS commentId, comment_post_ID AS commentPostId, comment_author AS commentAuthor, comment_author_email AS commentAuthorEmail, comment_author_url AS commentAuthorUrl, comment_author_IP AS commentAuthorIp, comment_date AS commentDate, comment_date_gmt AS commentDateGmt, comment_content AS commentContent, comment_karma AS commentKarma, comment_approved AS commentApproved, comment_agent AS commentAgent, comment_type AS commentType, comment_parent AS commentParent, user_id AS userId
    </sql>
    
    <select id="recentComments" resultMap="BaseResultMap">
    	SELECT comments.comment_author,posts.post_title FROM wp_comments AS comments LEFT JOIN wp_posts AS posts ON(comments.comment_post_ID=posts.ID) WHERE comments.comment_approved='0' AND posts.post_status='publish' ORDER BY comments.comment_date_gmt DESC LIMIT 0,5
    </select>

</mapper>

```


相关属性我就不做多的解释，关于MyBatis相关的教程，除了参考官网之外，还可以参考我的博客系列文章，地址为:https://www.cnblogs.com/youcong/category/1144041.html

关于ofType还是要说的，如果你的mybatis-config.xml或者是springboot中的application.yml或application.properties没有配置对应的别名，那么请将类的完整路径填写上去，假定我没有做出相关的配置的话，那么我需要这么写 ofType="com.blog.springboot.entity.Posts"。

collection的property要包含在com.blog.springboot.entity.Comments类里面

我贴出我的Comments类，大家可以做一个参考:
```
package com.blog.springboot.entity;

import java.io.Serializable;
import java.util.Date;
import java.util.List;

import com.baomidou.mybatisplus.activerecord.Model;
import com.baomidou.mybatisplus.annotations.TableField;
import com.baomidou.mybatisplus.annotations.TableId;
import com.baomidou.mybatisplus.annotations.TableName;
import com.baomidou.mybatisplus.enums.IdType;

/**
 * <p>
 * 
 * </p>
 *
 * @author youcong
 * @since 2019-02-12
 */
@TableName("wp_comments")
public class Comments extends Model<Comments> {

    private static final long serialVersionUID = 1L;

    @TableId(value = "comment_ID", type = IdType.AUTO)
    private Long commentId;
    @TableField("comment_post_ID")
    private Long commentPostId;
    @TableField("comment_author")
    private String commentAuthor;
    @TableField("comment_author_email")
    private String commentAuthorEmail;
    @TableField("comment_author_url")
    private String commentAuthorUrl;
    @TableField("comment_author_IP")
    private String commentAuthorIp;
    @TableField("comment_date")
    private Date commentDate;
    @TableField("comment_date_gmt")
    private Date commentDateGmt;
    @TableField("comment_content")
    private String commentContent;
    @TableField("comment_karma")
    private Integer commentKarma;
    @TableField("comment_approved")
    private String commentApproved;
    @TableField("comment_agent")
    private String commentAgent;
    @TableField("comment_type")
    private String commentType;
    @TableField("comment_parent")
    private Long commentParent;
    @TableField("user_id")
    private Long userId;
    
    @TableField(exist=false)
    private List<Posts> posts;
    

    public List<Posts> getPosts() {
		return posts;
	}

	public void setPosts(List<Posts> posts) {
		this.posts = posts;
	}

	public Long getCommentId() {
        return commentId;
    }

    public void setCommentId(Long commentId) {
        this.commentId = commentId;
    }

    public Long getCommentPostId() {
        return commentPostId;
    }

    public void setCommentPostId(Long commentPostId) {
        this.commentPostId = commentPostId;
    }

    public String getCommentAuthor() {
        return commentAuthor;
    }

    public void setCommentAuthor(String commentAuthor) {
        this.commentAuthor = commentAuthor;
    }

    public String getCommentAuthorEmail() {
        return commentAuthorEmail;
    }

    public void setCommentAuthorEmail(String commentAuthorEmail) {
        this.commentAuthorEmail = commentAuthorEmail;
    }

    public String getCommentAuthorUrl() {
        return commentAuthorUrl;
    }

    public void setCommentAuthorUrl(String commentAuthorUrl) {
        this.commentAuthorUrl = commentAuthorUrl;
    }

    public String getCommentAuthorIp() {
        return commentAuthorIp;
    }

    public void setCommentAuthorIp(String commentAuthorIp) {
        this.commentAuthorIp = commentAuthorIp;
    }

    public Date getCommentDate() {
        return commentDate;
    }

    public void setCommentDate(Date commentDate) {
        this.commentDate = commentDate;
    }

    public Date getCommentDateGmt() {
        return commentDateGmt;
    }

    public void setCommentDateGmt(Date commentDateGmt) {
        this.commentDateGmt = commentDateGmt;
    }

    public String getCommentContent() {
        return commentContent;
    }

    public void setCommentContent(String commentContent) {
        this.commentContent = commentContent;
    }

    public Integer getCommentKarma() {
        return commentKarma;
    }

    public void setCommentKarma(Integer commentKarma) {
        this.commentKarma = commentKarma;
    }

    public String getCommentApproved() {
        return commentApproved;
    }

    public void setCommentApproved(String commentApproved) {
        this.commentApproved = commentApproved;
    }

    public String getCommentAgent() {
        return commentAgent;
    }

    public void setCommentAgent(String commentAgent) {
        this.commentAgent = commentAgent;
    }

    public String getCommentType() {
        return commentType;
    }

    public void setCommentType(String commentType) {
        this.commentType = commentType;
    }

    public Long getCommentParent() {
        return commentParent;
    }

    public void setCommentParent(Long commentParent) {
        this.commentParent = commentParent;
    }

    public Long getUserId() {
        return userId;
    }

    public void setUserId(Long userId) {
        this.userId = userId;
    }

    @Override
    protected Serializable pkVal() {
        return this.commentId;
    }

    @Override
    public String toString() {
        return "Comments{" +
        ", commentId=" + commentId +
        ", commentPostId=" + commentPostId +
        ", commentAuthor=" + commentAuthor +
        ", commentAuthorEmail=" + commentAuthorEmail +
        ", commentAuthorUrl=" + commentAuthorUrl +
        ", commentAuthorIp=" + commentAuthorIp +
        ", commentDate=" + commentDate +
        ", commentDateGmt=" + commentDateGmt +
        ", commentContent=" + commentContent +
        ", commentKarma=" + commentKarma +
        ", commentApproved=" + commentApproved +
        ", commentAgent=" + commentAgent +
        ", commentType=" + commentType +
        ", commentParent=" + commentParent +
        ", userId=" + userId +
        "}";
    }
}

```

也许大家发现我的mybatis与你们的mybatis不一样，实际上我用的是mybatis-plus，mybatis-plus可以说跟mybatis几乎没有什么区别，我多次强调过，mybatis-plus是mybatis的增强版，意味着mybatis原有的功能，mybatis-plus可以毫无顾忌的拿来即用。

关于mybatis-plus的学习教程，感兴趣的朋友可以参考我的这篇博客(包含从入门到使用):https://www.cnblogs.com/youcong/category/1213059.html

## sql优化策略

sql优化的策略有很多，大家可以参考如下:

(1)任何地方都不要使用select * from table_name，请使用具体的字段列表代替"*" ，不要返回用不到的任何字段;
(2)对查询进行优化，应尽量避免全表扫描，首先应考虑在where及order by涉及的列建立索引;
(3)应尽量避免在where子句中使用or来连接条件，否则将导致引擎放弃使用索引而进行全表扫描;
(4)应尽量避免在where子句中使用!=或<>操作符，否则将导致引擎放弃使用索引而进行全表扫描;
(5)int和not in慎用，否则会导致全表扫描;
(6)应尽量避免在where子句中对字段进行表达式操作，这将导致引擎放弃使用索引而进行全表扫描;
(7)很多时候使用exists代替in是一个好的选择;
(8)尽量使用数字型字段，若只含数值信息的字段设计为字符型，这将会降低查询和连接的性能，并会增加存储开销,这是因为引擎在处理查询和连接时会逐个比较字符串职工的每一个字符，而对于数字型而言只需要比较一次就够了;
(9)尽可能使用varchar代替char，因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些;

当然远远不止这么多，知识的海洋是无穷的，探索的乐趣亦如此。

关于sql优化思路，大家可以参考[SQL优化思路大全](https://www.cnblogs.com/wcwen1990/p/7204739.html)

