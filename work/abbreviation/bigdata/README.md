Maven 构建工具（把项目从头-编译-打包-部署）

Hadoop是一个开源的分布式计算框架，是生态框架，除了包含HDFS，还包含MapReduce、YARN、Hive、Pig、Spark
HDFS（Hadoop Distributed File System）是Hadoop中用于存储和管理大规模数据的分布式文件系统
Hive是数据仓库软件，它提供了一种类似于SQL的查询语言，称为HiveQL，用于查询和分析存储在HDFS中的数据。Hive将Hadoop中的MapReduce作为底层计算引擎，通过执行HiveQL查询来将MapReduce作业转换为分布式任务，从而处理大规模数据。Hive的主要目标是使数据分析师和开发人员能够轻松地使用SQL语言来查询和分析存储在Hadoop集群中的数据，而无需了解复杂的MapReduce编程


HDFS是一个分布式文件系统：传统文件系统的硬盘寻址慢，通过引入存放文件信息的服务器Namenode和实际存放数据的服务器Datanode进行串接。对数据系统进行分布式储存读取。
MapReduce是一个计算框架/编程模型：MapReduce的核心思想是把计算任务分配给集群内的服务器里执行。通过对计算任务的拆分（Map计算\Reduce计算）再根据任务调度器（JobTracker）对任务进行分布式计算。大规模并行运算
Map映射
Reduce归约
YARN（资源管理系统）
HBase（分布式、多版本、无模式、稀疏、面向列的开源数据库），跟hive相比有啥区别？更贵？可实时修改？分区是谁的概念？
利用hadoop HDFS作为文件存储系统，
利用Zookeeper作为协同服务，
利用MapReduce处理HBase中的海量
利用Hive挖掘HBase中的海量，将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行
利用Spark分析HBase中的海量



apache 基金会，下面有很多开源项目

hadoop：Hadoop是一个生态系统基于java开发的开源平台，包括HDFS（存储）、MapReduce（计算）、Yarn（资源调度）。一个大脑加一个口袋构成一个单体，大脑负责计算数据，口袋负责存储数据。多个单体构成集群。

hive：基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射成一张虚表，并提供类SQL查询功能；内部是转化为MapReduce程序。没有服务端，它本质是Hadoop或者说是HDFS的一个客户端，对HDFS的数据和Meta store的元数据进行操作。有了这个mapreduce让多个大脑同时计算存储在多个口袋里的数据。

spark:是一个用scala(开发语言)编写的计算框架，基于内存的快速、通用、可扩展的大数据分析引擎，对标hadoop，比hadoop快

flink。。和spark也对标