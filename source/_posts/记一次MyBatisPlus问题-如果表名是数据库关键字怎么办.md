---
title: 记一次MyBatisPlus问题(如果表名是数据库关键字怎么办)
date: 2019-08-30 15:58:35
tags: "MyBatis-Plus"
---


问题信息:
如果表名是数据库关键字怎么办？

正常来说，如果是我们自己写sql的话，给表名加反引号即可解决问题。

但是由于我们使用MyBatisPlus，相关的sql基本上都是封装并自动生成的。如果是这种场景，我们就需要修改对应的实体，举例说明，如下代码:
<!--more-->
```
import com.baomidou.mybatisplus.enums.IdType;
import com.baomidou.mybatisplus.annotations.TableId;
import com.baomidou.mybatisplus.activerecord.Model;
import java.io.Serializable;

public class Group extends Model<Group> {

    private static final long serialVersionUID = 1L;

    /**
     * 组ID
     */
    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    /**
     * 组名
     */
    private String name;


    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    protected Serializable pkVal() {
        return this.id;
    }

    @Override
    public String toString() {
        return "Group{" +
        "id=" + id +
        ", name=" + name +
        "}";
    }
}


```

用上述代码的自动生成肯定会有问题，以单条数据查询为例，默认是 select id,name from group where id = 1，又因为group属于关键字，接下来会出现如下错误信息:
```
### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'group WHERE id=1' at line 1
; bad SQL grammar []; nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'group WHERE id=1' at line 1] with root cause

com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'group WHERE id=1' at line 1

```

这种错误信息，很容易识别，一看就是sql写的有问题。但实际上sql并没有问题，只不过是因为关键字冲突导致sql错误。

那么如何解决这个问题呢？
答案是只需加一个@TableName注解即可解决该问题。修改后的实体代码如下:
```
import com.baomidou.mybatisplus.enums.IdType;
import com.baomidou.mybatisplus.annotations.TableId;
import com.baomidou.mybatisplus.annotations.TableName;
import com.baomidou.mybatisplus.activerecord.Model;
import java.io.Serializable;


@TableName("`group`")
public class Group extends Model<Group> {

    private static final long serialVersionUID = 1L;

    /**
     * 组ID
     */
    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    /**
     * 组名
     */
    private String name;


    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    protected Serializable pkVal() {
        return this.id;
    }

    @Override
    public String toString() {
        return "Group{" +
        "id=" + id +
        ", name=" + name +
        "}";
    }
}


```