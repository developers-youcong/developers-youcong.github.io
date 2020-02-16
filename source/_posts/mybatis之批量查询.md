---
title: mybatis之批量查询
date: 2019-09-10 17:30:56
tags: "MyBatis"
---

关于MyBatis批量更新和添加，参考我的如下文章即可:
[MyBatis的批量更新实例](https://www.cnblogs.com/youcong/p/8476441.html)

[MyBatis的批量添加实例](https://www.cnblogs.com/youcong/p/9356776.html)

另外不管是批量的新增、删除、修改、查询也好，还是单个新增、删除、修改查询也罢。都会用到动态SQL。

关于MyBatis的动态SQL可以参考我的这篇文章，如下:
[MyBatis实战之动态SQL](https://www.cnblogs.com/youcong/p/8476441.html)

今天这篇文章主要是为了记录，最近用MyBatis-Plus特别多，很多增、删、改、查以及批量相关操作，拿来即用，戊戌时自己编写。特轻松。

但是因为最近的一个需求不得不自己手写批量查询例子(主要涉及联表之类的操作)。

正好以该例子进行讲解，也给我，给大家做个小小参考。

<!--more-->
关键XML:
```
	<select id="getStudentSubmitHomeWorkListInfos" resultMap="BaseResultMap">

	SELECT s.`solution_id`,s.`problem_id`,s.`user_id`,s.`nick`,s.`result`,p.title
	FROM solution AS s left join problem as p ON(s.problem_id = p.problem_id) WHERE  
	 s.`user_id` in
    <foreach collection="list" item="userId" open="(" close=")" separator=",">
    #{userId}
    </foreach>

	</select>

```

foreach相关参数解释:

collection配置的users是传递进来的参数名称，它可以是一个数组或者List、Set等集合;

item配置的是循环中当前的元素;

index配置的是当前元素在集合的位置下标;

separator是各个元素的间隔符;

open和colose代表的是以什么符号将元素包裹起来;

关键DAO:
```
	public List<Solution> getStudentSubmitHomeWorkListInfos(List<String> userId);

```


单元测试:
```
    @Test
	public void testCollectionRun() {
		
		List<String> userId = new ArrayList<String>();
		userId.add("admin");
		userId.add("student");

	
 		
		List<Solution> solutionList = solutionDao.getStudentSubmitHomeWorkListInfos(userId);		
		for (Solution solution : solutionList) {
			
			System.out.println("solution:"+solution.getNick()+"||"+solution.getResult()+"||"+solution.getTitle());
		}
		
		
		
	}

```



顺便说说批量查询的应用场景:
(1)特定的场景获取用户订单列表数;
(2)获取某一个题目许学生提交的结果(以用户id作为查询参数，该用户id非主键);

