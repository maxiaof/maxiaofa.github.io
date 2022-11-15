+++
title = "MyBatis一对多分页"
chaper = true
+++

# 一对多分页(适用于一对一)

## 1.1 实例SQL

```sql
SELECT
	* 
FROM
	tab_1 t1
	LEFT JOIN tab_2 t2 ON t1.id = t2.t1_id 
	LIMIT 0,10
-- 当第一个用户有十条地址数据,那么上面的查询0,10,刚好是第一个用户的分页完成,此时再进行一对多封装,将会导致封装之后,仅有一个用户数据,分页不准确

```

## 1.2 解决办法

```xml
<!--mybatis实体映射xml中如下配制-->
<resultMap>
    ...
     <collection property="t2"  ofType="tab2"
                    select="selectTab2List" column="{t2_id=id}"/>
</resultMap>
```

```
其中关键点有两个

1.select=“selectTab2List”
指定嵌套语句的具体执行语句id

2. column="{t2_id=id}"
指定嵌套语句的关联条件,我将主表的字段id,起了一个别名叫做t2_id
```

