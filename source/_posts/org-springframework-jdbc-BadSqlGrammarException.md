---
title: org.springframework.jdbc.BadSqlGrammarException
date: 2019-03-28 23:04:08
tags: "MyBatis"
---
这个错误在MyBatis中实际上很常见，就是SQL写错了。通常通过先在MySQL命令行执行一遍sql看有没有错误，如果有就更改，没有就基本上可以用了。
注意，我说的基本上可用并不代代表完全可用，比如今天我就遇到一个非常恶心的问题。
<!--more-->
sql代码如下(这句sql经过在mysql命令行中测试，能够获取数据，完全没有问题):
```
	<select id="categoreListInfo" resultMap="BaseResultMap">
		SELECT
		term.term_id,term.name,
		term.slug,tax.taxonomy,tax.description,tax.count FROM wp_terms AS term
		LEFT JOIN wp_term_taxonomy AS tax ON(term.term_id=tax.term_id)
		<where>
			<if test="name != null or name != ''">
				and term.name like concat('%', #{name}, '%')
			</if>
			<if test ="taxonomy != null or  taxonomy !=''">
			    and tax.taxonomy = #{taxonomy}
			</if>
		</where>
		limit #{start},#{size}
	</select>

```

但是我用如下单元测试就出现了问题，单元测试代码如下:
```
    @Test
	public void testPageListInfo() throws Exception {
		
		Map<String,Object> paramMap = new HashMap<String,Object>();
		paramMap.put("name", "");
		paramMap.put("start","0");
		paramMap.put("size", "10");
		paramMap.put("taxonomy", "post_tag");
		
		
		List<Terms> list = termService.categoreListInfo(paramMap);
		
		int count = termService.categoryTotalCount(paramMap);
		
		System.out.println("总数:"+count);
		
		for (Terms terms : list) {
			
			System.out.println("terms:"+terms.getName());
			
			List<TermTaxonomy> taxList = terms.getTax();
			
			for (TermTaxonomy termTaxonomy : taxList) {
				
				System.out.println("tax:"+termTaxonomy.getDescription()+"||"+termTaxonomy.getTaxonomy());
			}
		}
		
	}

```

单元测试并没有写错，错的是参数问题，关键点是这个:
```
		paramMap.put("start","0");
		paramMap.put("size", "10");

```

在mysql中limit两个参数实际为int类型，非字符串，而我此时在此传字符串，所以就出现org.springframework.jdbc.BadSqlGrammarException,说sql有问题。

所以大家切记在命令行执行sql以后，千万别掉以轻心，还是要细心，否则不必要的错误非常让人糟心。

