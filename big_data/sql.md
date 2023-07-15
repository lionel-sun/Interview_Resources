# ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `SQL题`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `collect_set()/collect_list()/concat_ws()`

set去重，list不去重。concat_ws(',',collect_set(name)) as name_set

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `解析json`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `lateral view explode用法`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `窗口函数`

窗口函数的基本语法如下：<窗口函数> over (partition by <用于分组的列名> order by <用于排序的列名>)
行限制语法：rows between ... and ...（range between事从数值上限制。）
- unbounded preceding 前面所有
- unbounded following 后面所有
- current row 当前行
- n following 后n行
- n preceding 前n行

排序函数：

- rank() 1，1，1，4，5，
- dense_rank() 1，1，1，2，3，
- row_number() 1，2，3，4，5，

聚合窗口函数：SUM、AVG、MAX、MIN。都是根据order by列名进行排序，对当前所在行之上的进行聚合。

lead(字段值,行数,默认值)
lag(字段值,行数,默认值)

lead(date,9,null) OVER(Partition BY city Order BY date ASC) AS data_p9d

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `聚合操作（统计连续10天在线超过1000人的城市）`

table logs
user_id, city, date

```sql
select 
	t3.city
from
(select 
	t2.city, t2.uid
from
(select
	t1.city, t1.user_id, date_sub(t1.date, t1.rnk) as dt
from 
	(select 
		l.city, l.user_id, l.date, row_number() over(partition by l.user_id order by l.date) as rnk
	from 
		logs as l) as t1) as t2
group by
	t2.city, t2.user_id, t2.dt
having 
	count(t2.dt) > 10) as t3
group by
	t3.city
having 
	count(t3.user_id) > 1000
```

```sql
select b.city
from 
(select a.city, count(distinct a.user_id) as count, a.date - ROW_NUMBER() OVER(PARTITION BY a.city ORDER BY a.date) AS temp 
from
(select t.*
from user_data t
order by t.user_id, t.date
) a
gouryby b.city
having count>=1000
) b
where b.temp >=10
```


### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `with as用法和子句排序`

CTE 语法的with as用法和子句使用的顺序。CTE可以引用自身，和同一个with下已经定义的，但是不能引用后面的避免互相嵌套递归。

```sql
with 
	t1 as (
		select 
		from
	)
	t2 as (
		select 
		from t1
	)
select
	t2.uid, count(t2.uid)
from
	t2
where
	t2.uid <> s
group by
	t2.uid
having
	count(t2.uid) > 10
order by
	count(uid) desc
limit 
	1, 3
# 第2开始显示3个
```

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `行转列，列转行函数`

pivot和unpivot

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `将一列根据分隔符分割成多列`

SUBSTRING_INDEX(str,delim,count)  -- str: 被分割的字符串; delim: 分隔符; count: 分割符出现的次数。

count 为整数标识从左边开始，count为负数从右边开始。

```sql

select gmt_create
,(select substring_index(substring_index(bill_ids,',',1),',',-1)) bill_id1
,(select substring_index(substring_index(bill_ids,',',2),',',-1)) bill_id2
,(select substring_index(substring_index(bill_ids,',',3),',',-1)) bill_id3
from lt_repayment;

```

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `直播统计`

1、统计每天每个直播间的访客数、每天最大访客数的直播间
2、查找至少连续观看3天的用户ID 及出现直播间

```sql

select LiveID,count(UserID) as visitnum 
FROM table
Group By LiveID 
Order By visitnum DESC(ASC)

select max(visitnum) as maxvisit,LiveID 
From previous table

SELECT *
from (select Date - ROW_NUMBER() OVER(ORDER BY DATE ASC) AS times,userid, liveid
FROM table
Group By userid, liveid) as T
where times>3


```

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `如何删除两条一模一样的数据中的一条`

创建视图在视图中删除，视图中没有聚合的操作是可以删除的，删除原表数据。

name, value
```sql
DELETE FROM
(SELECT ROW_NUMBER() OVER(PARTITION BY name, value ORDER BY(SELECT 1)) AS no,name, value 
FROM test_table
) AS T WHERE T.no != 1
```

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `leetcode SQL题`

#### [leetode sql题库](https://leetcode-cn.com/problemset/all/?search=sql)

#### [删除重复的电子邮箱](https://leetcode-cn.com/problems/delete-duplicate-emails/)

```sql
delete p1
from Person p1, Person p2
where p1.Email = p2.Email and p1.Id > p2.Id;
```

#### [行程和用户](https://leetcode-cn.com/problems/trips-and-users/)

```sql
select 
    t.Request_at as Day,
    round(
        sum(if(t.Status = 'completed',0,1))/count(t.Status),
        2
    ) as 'Cancellation Rate'
from
    Trips as t
    inner join Users as u1 on t.Client_Id = u1.Users_Id and u1.Banned = 'No'
    inner join Users as u2 on t.Driver_Id = u2.Users_Id and u2.Banned = 'No'
where 
    t.Request_at between '2013-10-01' and '2013-10-03'
group by
    t.Request_at
order by
    t.Request_at asc
```

#### [部门工资前三高的所有员工](https://leetcode-cn.com/problems/department-top-three-salaries/)

```sql
select 
    d.name as department,
    e.name as employee,
    e.salary
from 
    employee as e,
    department as d,
    (select
        id, dense_rank() over(partition by departmentid order by salary desc) as rrank
    from 
        employee) as c
where 
    e.departmentid = d.id and e.id = c.id and c.rrank <4
```

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `牛客 SQL题`

#### [nowcoder sql题库](https://www.nowcoder.com/ta/sql)
