---
title: 'org.xml.sax.SAXParseException;在实体引用中, 实体名称必须紧跟在 ''&'' 后面'
date: 2019-03-28 22:59:08
tags: "MyBatis"
---
错误信息如下:
org.xml.sax.SAXParseException;在实体引用中, 实体名称必须紧跟在 ''&'' 后面

出现这个错误的原因是在xml中使用&，实际上xml中不支持这种方式，&其实是并列的意思，如果要在xml中使用&，需要将其改为英文 and才能使用。
<!--more-->
问题代码:
```
	<select id="categoreListInfo" resultMap="BaseResultMap">
		SELECT
		term.term_id,term.name,
		term.slug,tax.taxonomy,tax.description,tax.count FROM wp_terms AS term
		LEFT JOIN wp_term_taxonomy AS tax ON(term.term_id=tax.term_id)
		<where>
			<if test="name != null & name != ''">
				and term.name like concat('%', #{name}, '%')
			</if>
			<if test ="taxonomy != null & taxonomy !=''">
			    and tax.taxonomy = #{taxonomy}
			</if>
		</where>
		limit #{start},#{size}
	</select>

```

将代码中的&条件改为and即可

参考资料:
【java】org.xml.sax.SAXParseException;在实体引用中, 实体名称必须紧跟在 '&' 后面。解决方法:https://blog.csdn.net/demon_ll/article/details/78542356