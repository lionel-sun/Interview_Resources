# 数据研发相关

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `数据库`

### [范式](https://zhuanlan.zhihu.com/p/63146817

1NF 属性不可分，确保每列保持原子性（地址中拆分出城市）

2NF 每个非主属性完全函数依赖于键码。可以通过分解来满足。(确保表中的每列都和主键相关)
一个表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中。

3NF 非主属性不传递函数依赖于键码。(确保每列都和主键列直接相关,而不是间接相关)

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `事务ACID`

事务指的是满足 ACID 特性的一组操作，可以通过 Commit 提交一个事务，也可以使用 Rollback 进行回滚。
原子性（Atomicity）一致性（Consistency）隔离性（Isolation）一个事务最终提交前，其他事务不可见。持久性（Durability）

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `事务隔离级别，按照性能排序`

未提交读（READ UNCOMMITTED）提交读（READ COMMITTED）可重复读（REPEATABLE READ）可串行化（SERIALIZABLE）

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `存储过程`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `红黑树`

自平衡的二叉查找树，有序（有序输出比较快），红黑树通过规则设置保证了查找和删除的时间复杂度都是O(logn)，
三种自平衡操作：左旋、右旋和变色。红黑树多用于内存中查询。

性质1：每个节点要么是黑色，要么是红色。
性质2：根节点是黑色。
性质3：每个叶子节点（NIL）是黑色。
性质4：每个红色结点的两个子结点一定都是黑色。
性质5：任意一结点到每个叶子结点的路径都包含数量相同的黑结点。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `B树`

B树平衡多路查找树（m阶）：每个节点最多有m-1个关键字（可以存有的键值对）。根节点最少可以只有1个关键字。
非根节点至少有m/2个关键字。每个节点中的关键字都按照从小到大的顺序排列，每个关键字的左子树中的所有关键字都小于它，而右子树中的所有关键字都大于它。
所有叶子节点都位于同一层，或者说根节点到每个叶子节点的长度都相同。每个节点都存有索引和数据，也就是对应的key和value。
M阶树，插入时当关键字大于M-1的时候要进行节点拆分。
删除时候当父亲节点关键字小于2，从右侧选取放到父。从父挪一个到当前位置。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `B+树`

B+树有两种类型的节点：内部节点（也称索引结点）和叶子结点。内部节点就是非叶子节点，内部节点不存储数据，只存储索引，数据都存储在叶子节点。
根节点至少一个元素，非根节点元素范围：m/2 <= k <= m-1。
内部结点中的key都按照从小到大的顺序排列，对于内部结点中的一个key，左树中的所有key都小于它，右子树中的key都大于等于它。叶子结点中的记录也按照key的大小排列。
每个叶子结点都存有相邻叶子结点的指针，叶子结点本身依关键字的大小自小而大顺序链接。父节点存有右孩子的第一个元素的索引。


### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `红黑树、B+树和B树区别`

B树的优点：对于在内部节点的数据，可直接得到，不必根据叶子节点来定位。

B+树的优点：多用于外存中，磁盘读写代价低（索引可以都放在一个block中，硬盘寻址次数和树高成正比，寻址次数降低）。
查询效率稳定。叶子节点之间通过指针来连接，遍历所有数据更方便（B树需要中序遍历）。

红黑树多用在内部排序，即全放在内存中的。比B树高更高。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `mysql的哈希索引和B+树索引`

哈希无序，无法排序和模糊查找。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `内连接，左连接，右连接，全连接`

内连接只两边能连接在一起的数据返回，左连接返回左表数据并把右表关联的数据也连接上返回（一对多情况下右表中有多个结果也都返回）
全连接两边数据都展示，如果没有连接就是空值

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `唯一索引，索引的数据结构`

数据量大，减少服务器扫描数据量。帮助排序和分组（避免临时空间），随机I/O变为顺序I/O（B+树）

普通索引加快速度，唯一索引确定某个列的值是独一无二的。mysql插入时候会检查。通常不是用来提高速度是用来避免重复。主键索引也可以保证唯一

B+树和哈希的数据结构

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `概念问题`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `星型模型和雪花型模型比较`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `数据仓库、数据湖、数据中台`

数据仓库(Data Warehouse)是一个面向主题的（Subject Oriented）、集成的（Integrated）、相对稳定的（Non-Volatile）、反映历史变化的（Time Variant）数据集合，
用于支持管理决策和信息的全局共享。

数据湖（Data Lake）是一个存储企业的各种各样原始数据的大型仓库，其中的数据可供存取、处理、分析及传输。
数据湖是以其自然格式存储的数据的系统或存储库，通常是对象blob或文件。数据湖通常是企业所有数据的单一存储，
包括源系统数据的原始副本，以及用于报告、可视化、分析和机器学习等任务的转换数据。数据湖可以包括来自关系数据库（行和列）的结构化数据，
半结构化数据（CSV，日志，XML，JSON），非结构化数据（电子邮件，文档，PDF）和二进制数据（图像，音频，视频）。

数据中台是指通过企业内外部多源异构的数据采集、治理、建模、分析，应用，使数据对内优化管理提高业务，对外可以数据合作价值释放，
成为企业数据资产管理中枢。数据中台建立后，会形成数据API，为企业和客户提供高效各种数据服务。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `sybase IQ列式数据库和行式数据库区别`

列式数据库优点：速度快，适合大数据，实时加载数据仅限于增加（删除和更新需要解压缩Block 然后计算然后重新压缩储存），高效的压缩率，不仅节省储存空间也节省计算内存和CPU。
非常适合做聚合操作。定期批量更新（一天三次左右）

列式缺点：不适合小数据，不适合随机更新，不适合做删除和实时操作的。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `数据倾斜怎么办？`

Hive数据运算卡在99.9%，SparkStreaming做实时算法时候，可能会出现OOM，但其他的executor使用率低。关键是数据量大。
产生的原因是Shuffle动作中，相同key的值就会拉到一个或几个节点上，就容易发生单点问题。就是数据的key 的分化严重不均

Hadoop：reduce卡住，container报错OOM，出现任务被kill等诡异情况。Hive经常出现在sql的group by上。
mapjoin方法（关联操作中有一张小表，将小表直接读入内存，在map阶段直接拿另外一张表和内存中的表做计算，计算结果给reduce，提高了reduce的效率）

Hive参数调优：

set hive.map.aggr=true；在map中会做部分聚集操作，效率更高但需要更多的内存。

set hive.groupby.skewindata=true;数据倾斜时负载均衡，当选项设定为true，生成的查询计划会有两个MRJob。

第一个MRjob，Map的输出结果集合会随机分布到Reduce中，每个Reduce（相同key可能会发到不同reduce中）做部分聚合操作
第二个MRjob，再根据key分到不同reduce中

在key上修改，aaa（大量数据的）加上后缀氛围1，2，3，4，5等，第一次计算后恢复在第二次计算。

先group操作把key进行一次reduce，然后在进行count或者distinct count

Spark：Streaming和sql上。Executor执行时间特别久，整体任务卡在某个阶段不能结束。streaming中一些类似sql的join、group操作会经常出现数据倾斜。
mapjoin方法（同上）

数据角度：异常数据直接删除（例如ip为0），对不均匀的数据单独计算（如北京数据非常多，单独计算后和其他城市合并），使用hash将可以将key打散，然后再汇总。数据预处理


## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `Hadoop`
> MR，HDFS等基本概念，面试常见题型

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `HDFS基本结构`

分布式文件系统HDFS是大数据的存储，block块默认最多可以存储128M

HDFS集群包括，NameNode和DataNode以及Secondary Namenode。NameNode负责管理整个文件系统的元数据，以及每一个路径（文件）所对应的数据块信息。
DataNode 负责管理用户的文件数据块，每一个数据块都可以在多个datanode上存储多个副本。
Secondary NameNode用来监控HDFS状态的辅助后台程序，每隔一段时间获取HDFS元数据的快照。最主要作用是辅助namenode管理元数据信息

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `HDFS优缺点`

优点：1，高容错性。多个副本,切丢失一个后自动恢复。2，适合批处理，通过移动计算而不是移动数据，把数据位置暴露给计算框架。
3，适合大数据处理，可以处理GB,TB,PB级数据，可以处理百万规模以上的文件数量和10K节点的规模。4，流式数据访问，一次写入，多次读取，不能修改，只能追加。一致性。
5，可构建在廉价机器上

缺点：1，不适合低延时数据访问；2，无法高效的对大量小文件进行存储。3，并发写入、文件随机修改，一个文件只能有一个写，不允许多个线程同时写。
仅支持数据 append（追加），不支持文件的随机修改。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `MR是什么`

是大数据的计算

MapReduce流程：input->Splitting->Mapping->Shuffling->Reducing-> result

Hadoop计算框架Shuffle处于map和reduce阶段之间。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `MR编程流程模型`

map两步：1，设置inputFormat类，将我们的数据切分成key，value对，输入到第二步。
2，自定义map逻辑，处理我们第一步的输入数据，然后转换成新的key，value对进行输出

shuffle四步：3，对输出的key，value对进行分区。相同key的数据发送到同一个reduce里面去，相同key合并，value形成一个集合。
4，对不同分区的数据按照相同的key进行排序。5，对分组后的数据进行规约(combine操作)，降低数据的网络拷贝（可选步骤）
6，排序后分组，相同key的value放到同一个集合。

reduce两步：1，对多个map的任务进行合并，排序，写reduce函数自己的逻辑，对输出key value合并处理转换新的key，value输出。
2，设置outputformat将输出的key，value对数据进行保存到文件中

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `wordcount讲解`

1读取数据，生成key，value（word， 1）2，按照key排序。3，合并（key，value1，value2。。。）4，汇聚结果输出key，value1+value2+value3

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `Hive`
> 基本概念，面试常见题型

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `什么是和Hive`

Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供类SQL查询功能。其本质是将SQL转换为MapReduce的任务进行运算，
底层由HDFS来提供数据的存储，说白了hive可以理解为一个将SQL转换为MapReduce的任务的工具，甚至更进一步可以说hive就是一个MapReduce的客户端

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `与数据库的区别`

Hive 具有 SQL 数据库的外表，但应用场景完全不同。Hive 只适合用来做海量离线数据统计分析，也就是数据仓库。Hive主要是批量操作，执行延迟高。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `Hive的优缺点`

优点：1，操作接口采用类SQL语法，提供快速开发的能力（简单、容易上手）。
2，避免了去写MapReduce，减少开发人员的学习成本。
3，Hive支持用户自定义函数，用户可以根据自己的需求来实现自己的函数。

缺点：1，Hive 的查询延迟很严重

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `Spark`
> 基本概念，面试常见题型

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `Spark为什么比MR更快`

MapReduce通常需要将计算的中间结果写入磁盘，然后还要读取磁盘，从而导致了频繁的磁盘IO。
Spark则不需要将计算的中间结果写入磁盘，这得益于Spark的RDD（弹性分布式数据集，很强大）和DAG（有向无环图），
其中DAG记录了job的stage以及在job执行过程中父RDD和子RDD之间的依赖关系。中间结果能够以RDD的形式存放在内存中，且能够从DAG中恢复，大大减少了磁盘IO。

MapReduce在Shuffle时需要花费大量时间进行排序，排序在MapReduce的Shuffle中似乎是不可避免的；
Spark在Shuffle时则只有部分场景才需要排序，支持基于Hash的分布式聚合，更加省时；

MapReduce采用了多进程模型，多进程模型的好处是便于细粒度控制每个任务占用的资源，但每次任务的启动都会消耗一定的启动时间。
Spark则是通过复用线程池中的线程来减少启动、关闭task所需要的开销。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `处理spark oom`

大约有两个情况：1、map执行内存溢出.2、shuffle后内存溢出

map执行中内存溢出代表了所有map类型的操作。包括：flatMap，filter，mapPatitions等。
通过减少每个task的大小来减少Executor内存中的数量，具体做法是在调用map操作前先调用repartition方法，增大分区数来减少每个分区的大小，再传入map中进行操作。

shuffle后内存溢出的shuffle操作包括join，reduceByKey，repartition等操作。
shuffle内存溢出的情况可以说都是shuffle后，shuffle会产生数据倾斜，少数的key内存非常的大，它们都在同一个Executor中计算，导致运算量加大甚至会产生OOM。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `spark的rdd特性`

弹性分布式数据集，是Spark中最基本的数据抽象，它代表一个不可变、可分区、里面的元素可并行计算的集合.
Dataset: 就是一个集合，存储很多数据.
Distributed：它内部的元素进行了分布式存储，方便于后期进行分布式计算.
Resilient： 表示弹性，rdd的数据是可以保存在内存或者是磁盘中.

五大属性：1，一个分区（Partition）列表，数据集的基本组成单位。一个rdd有很多分区（包含了该rdd的部分数据）
spark中任务是以task线程的方式运行， 一个分区就对应一个task线程。
2，一个计算每个分区的函数。Spark中RDD的计算是以分区为单位的，每个RDD都会实现compute计算函数以达到这个目的.
3，一个rdd会依赖于其他多个rdd。这里就涉及到rdd与rdd之间的依赖关系，spark任务的容错机制就是根据这个特性（血统）而来。
4，一个Partitioner，即RDD的分区函数（可选项）
5，一个列表，存储每个Partition的优先位置(可选项)

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `spark和flink`

flink提供事件级处理，也称为实时流。Spark是迷你批处理。这种方法被称为接近实时。

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `SQL题`

## ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `聚合操作（统计连续10天在线超过1000人的城市）`

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


```sql

```

## 引用资源

- [大数据面试题总结](https://www.jianshu.com/p/045a576abeea)

- [阿里，头条，美团，快手大数据开发岗面试总结](https://mp.weixin.qq.com/s/SHH64TJvx5kpIl-3cMzypw)
