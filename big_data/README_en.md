# Data Engineer English

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `DW modeling`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `Kimball维度建模方法`

维度建模步骤：选择业务过程->声明粒度->确定维度->确定事实

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `维度建模模型的分类`

维度建模，DV建模（减少存储）

- 星型模型 事实-维度
- 雪花模型 事实-维度-维度
- 星座模型 事实-维度-事实 宽表都是

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `维度建模目的`

访问性能，数据成本，使用效率，数据质量

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `Hive & Batch`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `hive和数据库的区别`

- 优点：sql快速上手，不用写mr。支持自定义函数
- 缺点：时延高，不能单行的update，delete，insert不好。批量处理

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `hql 提交到yarn的流程`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `hql 的mapper`

map和reduce限制大小计算map和reduce的个数。高窄表减少map大小增加数量。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `Hive调优？`

- 结构调优
- 增量merge，冷热分治
- 优先列剪枝和过滤
- 小文件合并（平台强制）
- 避免重复扫描（临时表改view）
- mapjoin，调整参数，MBFJ

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `数据倾斜怎么办？`

- 查看执行流图，谁倾斜治理谁，打散key。distinc先group by再count。
- 调参调优：
  - set hive.map.aggr=true；在map中会做部分聚集操作，效率更高但需要更多的内存。
  - set hive.groupby.skewindata=true;数据倾斜时负载均衡，当选项设定为true，生成的查询计划会有两个MRJob。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `udf,udaf,udtf之间的区别`

UDF：用户定义（普通）函数，只对单行数值产生作用；单行进入，单行输出

UDAF：User- Defined Aggregation Funcation；用户定义聚合函数，可对多行数据产生作用；等同与SQL中常用的SUM()，AVG()，也是聚合函数；多行进入，单行输出。

UDTF：User-Defined Table-Generating Functions，用户定义表生成函数，用来解决输入一行输出多行；多行进入，多行输出。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `HDFS基本结构`

NameNode和DataNode以及Secondary Namenode（辅助管理）。

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `OLAP`

Druid, Clickhouse, Presto, Mysql

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `CK表引擎`

列式存储。不支持事务。

mergetree 有主键不去重，replacingmergetree分区内去重，时间不确定。

分布式表 in和gobal in的区别。

实时离线可以都写入，离线覆盖实时。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `CK，druid，presto对比`

- druid预聚合，基于时间列，大时间范围查询慢。去重不准。排序也不行。join也不行（spark实现）。都可实时离线。
- ck列式存储节省空间，分区性能好。运维比druid容易。（数据管控后续没用）灵活sql
- presto不支持实时，底层是hive，性能不稳定。

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `No Sql`

Hbase, Graph, ES

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `Hbase的compaction，读写操作的流程 ？`

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `Flink & Streaming`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) ``

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `Kafka`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) ``
