# 数据研发相关

## 概念问题

### 星型模型和雪花型模型比较

### 数据仓库、数据湖、数据中台

数据仓库(Data Warehouse)是一个面向主题的（Subject Oriented）、集成的（Integrated）、相对稳定的（Non-Volatile）、反映历史变化的（Time Variant）数据集合，
用于支持管理决策和信息的全局共享。

数据湖（Data Lake）是一个存储企业的各种各样原始数据的大型仓库，其中的数据可供存取、处理、分析及传输。
数据湖是以其自然格式存储的数据的系统或存储库，通常是对象blob或文件。数据湖通常是企业所有数据的单一存储，
包括源系统数据的原始副本，以及用于报告、可视化、分析和机器学习等任务的转换数据。数据湖可以包括来自关系数据库（行和列）的结构化数据，
半结构化数据（CSV，日志，XML，JSON），非结构化数据（电子邮件，文档，PDF）和二进制数据（图像，音频，视频）。

数据中台是指通过企业内外部多源异构的数据采集、治理、建模、分析，应用，使数据对内优化管理提高业务，对外可以数据合作价值释放，
成为企业数据资产管理中枢。数据中台建立后，会形成数据API，为企业和客户提供高效各种数据服务。

### sybase IQ列式数据库和行式数据库区别

列式数据库优点：速度快，适合大数据，实时加载数据仅限于增加（删除和更新需要解压缩Block 然后计算然后重新压缩储存），高效的压缩率，不仅节省储存空间也节省计算内存和CPU。
非常适合做聚合操作。定期批量更新（一天三次左右）

列式缺点：不适合小数据，不适合随机更新，不适合做删除和实时操作的。

## Mapreduce

### 编程流程模型

map两步：1，数据切分成key，value。2，自定义逻辑转换成新的key，value。

shuffle四步：3，按key分区，发送不懂reduce。4，不同分局按key排序。5，进行数据规约合并操作（可选）6，排序后分组，相同key放到同一个集合。

reduce两步：1，对map输出key value合并处理转换新的keyvalue。2，输出的值保存到文件中。

### wordcount讲解

1读取数据，生成key，value（word， 1）2，按照key排序。3，合并（key，value1，value2。。。）4，汇聚结果输出key，value1+value2+value3

## Hive


## Spark

## SQL题

### 直播统计

1、统计每天每个直播间的访客数、每天最大访客数的直播间
2、查找至少连续观看3天的用户ID 及出现直播间

```sql

select LiveID,count(UserID) as visitnum 
FROM table
Gourp By LiveID 
Order By visitnum DESC(ASC)

select max(visitnum) as maxvisit,LiveID 
From previous table

SELECT *
from (select Date - ROW_NUMBER() OVER(ORDER BY DATE ASC) AS times,userid, liveid
FROM table
Group By userid, liveid) as T
where times>3


```

## 引用资源

- [大数据面试题总结](https://www.jianshu.com/p/045a576abeea)

- [阿里，头条，美团，快手大数据开发岗面试总结](https://mp.weixin.qq.com/s/SHH64TJvx5kpIl-3cMzypw)
