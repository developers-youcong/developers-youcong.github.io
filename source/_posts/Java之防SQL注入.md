---
title: Java之防SQL注入
date: 2022-10-23 10:17:55
tags: "Java"
---

## 一、什么是SQL注入？
SQL注入（英语：SQL injection），也称SQL注入或SQL注码，是发生于应用程序与数据库层的安全漏洞。简而言之，是在输入的字符串之中注入SQL指令，在设计不良的程序当中忽略了字符检查，那么这些注入进去的恶意指令就会被数据库服务器误认为是正常的SQL指令而执行，因此遭到破坏或是入侵。
<!--more-->

有部分人认为SQL注入是只针对Microsoft SQL Server，但只要是支持处理SQL指令的数据库服务器，都有可能受到此种手法的攻击。


## 二、应用程序注入的高风险情况有哪些？
- 1.在应用程序中使用字符串联结方式或联合查询方式组合SQL指令。
- 2.在应用程序链接数据库时使用权限过大的账户（例如很多开发人员都喜欢用最高权限的系统管理员账户（如常见的root，sa等）连接数据库）。
- 3.在数据库中开放了不必要但权力过大的功能。
- 4.太过于信任用户所输入的资料，未限制输入的特殊字符，以及未对用户输入的资料做潜在指令的检查。

## 三、SQL注入的作用原理是什么？
- 1.SQL命令可查询、插入、更新、删除等，命令的串接。而以分号字符为不同命令的区别。（原本的作用是用于SubQuery或作为查询、插入、更新、删除……等的条件式）。
- 2.SQL命令对于传入的字符串参数是用单引号字符所包起来。（但连续2个单引号字符，在SQL数据库中，则视为字符串中的一个单引号字符）
- 3.SQL命令中，可以注入注解（连续2个减号字符 -- 后的文字为注解，或“/*”与“*/”所包起来的文字为注解）。
- 4.因此，如果在组合SQL的命令字符串时，未针对单引号字符作转义处理的话，将导致该字符变量在填入命令字符串时，被恶意窜改原本的SQL语法的作用。

## 四、SQL注入可能造成哪些伤害？
- 1.资料表中的资料外泄，例如企业及个人机密资料，账户资料，密码等。
- 2.数据结构被黑客探知，得以做进一步攻击（例如SELECT * FROM sys.tables）。
- 3.数据库服务器被攻击，系统管理员账户被窜改（例如ALTER LOGIN sa WITH PASSWORD='xxxxxx'）。
- 4.获取系统较高权限后，有可能得以在网页加入恶意链接、恶意代码以及Phishing等。
- 5.经由数据库服务器提供的操作系统支持，让黑客得以修改或控制操作系统。
- 6.攻击者利用数据库提供的各种功能操纵文件系统，写入Webshell，最终导致攻击者攻陷系统。
- 7.破坏硬盘资料，瘫痪全系统。
- 8.获取系统最高权限后，可针对企业内部的任一管理系统做大规模破坏，甚至让其企业倒闭。
- 9.网站主页被窜改，导致声誉受到损害。

## 五、在Java开发中如何避免SQL注入？

- 1.在使用MyBatis的时候，通常使用#{parm}作为传参能很大程度避免SQL注入。
- 2.针对Controller相关参数做严格校验与过滤也能很大程度避免SQL注入。


Java防SQL注入代码如下:
```
public class SqlUtil {

    /**
     * 定义常用的 sql关键字
     */
    public static String SQL_REGEX = "select |insert |delete |update |drop |count |exec |chr |mid |master |truncate |char |and |declare ";

    /**
     * 仅支持字母、数字、下划线、空格、逗号、小数点（支持多个字段排序）
     */
    public static String SQL_PATTERN = "[a-zA-Z0-9_\\ \\,\\.]+";

    /**
     * 检查字符，防止注入绕过
     */
    public static String escapeOrderBySql(String value) {
        if (StringUtil.isNotEmpty(value) && !isValidOrderBySql(value)) {
            throw new UtilException("参数不符合规范，不能进行查询");
        }
        return value;
    }

    /**
     * 验证 order by 语法是否符合规范
     */
    public static boolean isValidOrderBySql(String value) {
        return value.matches(SQL_PATTERN);
    }

    /**
     * SQL关键字检查
     */
    public static void filterKeyword(String value) {
        if (StringUtil.isEmpty(value)) {
            return;
        }
        String[] sqlKeywords = StringUtil.split(SQL_REGEX, "\\|");
        for (String sqlKeyword : sqlKeywords) {
            if (StringUtil.indexOfIgnoreCase(value, sqlKeyword) > -1) {
                throw new UtilException("参数存在SQL注入风险");
            }
        }
    }

}


```

完整源代码:
https://github.com/developers-youcong/yc-framework/blob/main/yc-common/yc-common-core/src/main/java/com/yc/common/core/base/utils/sql/SqlUtil.java