Maven 构建工具（把项目从头-编译-打包-部署）

Hadoop是一个开源的分布式计算框架，是生态框架，除了包含HDFS，还包含MapReduce、YARN、hive、Pig、Spark
HDFS（Hadoop Distributed File System）是Hadoop中用于存储和管理大规模数据的分布式文件系统
hive是数据仓库“工具”，它提供了一种类似于SQL的查询语言，称为hiveql，用于查询和分析存储在HDFS中的数据。hive将Hadoop中的MapReduce作为底层计算引擎，通过执行hiveql查询来将MapReduce作业转换为分布式任务，从而处理大规模数据。hive的主要目标是使数据分析师和开发人员能够轻松地使用SQL语言来查询和分析存储在Hadoop集群中的数据，而无需了解复杂的MapReduce编程；存储是行存储，通常以文本格式（如CSV或JSON）存储


HDFS是一个分布式文件系统：传统文件系统的硬盘寻址慢，通过引入存放文件信息的服务器Namenode和实际存放数据的服务器Datanode进行串接。对数据系统进行分布式储存读取。
MapReduce是一个计算框架/编程模型：MapReduce的核心思想是把计算任务分配给集群内的服务器里执行。通过对计算任务的拆分（Map计算\Reduce计算）再根据任务调度器（JobTracker）对任务进行分布式计算。大规模并行运算
Map映射
Reduce归约
YARN（资源管理系统）
hbase是分布式、面向列的“数据库”（跟hive相比更贵，因为提供实时读写吞吐能力），可以将相关的列分组存储在一起成列簇（Column Family），同一个列簇物理连续存储可以更高效地访问相关的列；

利用hadoop HDFS作为文件存储系统，
利用Zookeeper作为协同服务，
利用MapReduce处理hbase中的海量
利用hive挖掘hbase中的海量，将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行
利用Spark分析hbase中的海量



apache 基金会，下面有很多开源项目

hadoop：Hadoop是一个生态系统基于java开发的开源平台，包括HDFS（存储）、MapReduce（计算）、Yarn（资源调度）。一个大脑加一个口袋构成一个单体，大脑负责计算数据，口袋负责存储数据。多个单体构成集群。

spark:是一个用scala(开发语言)编写的计算框架，基于内存的快速、通用、可扩展的大数据分析引擎，对标hadoop，比hadoop快

flink。。和spark也对标