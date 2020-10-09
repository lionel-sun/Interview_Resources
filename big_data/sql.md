# ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `SQL题`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `窗口函数`

窗口函数的基本语法如下：<窗口函数> over (partition by <用于分组的列名> order by <用于排序的列名>)

排序函数：

- rank() 1，1，1，4，5，
- dense_rank() 1，1，1，2，3，
- row_number() 1，2，3，4，5，

聚合窗口函数：SUM、AVG、MAX、MIN。都是根据order by列名进行排序，对当前所在行之上的进行聚合。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `聚合操作（统计连续10天在线超过1000人的城市）`

user_id, city, date

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

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `行转列，列转行函数`

pivot和unpivot

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
(SELECT ROW_NUMBER() OVER(PARTITION BY name, value ORDER BY(SELECT 1)) AS no,
name, value FROM test_table
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
