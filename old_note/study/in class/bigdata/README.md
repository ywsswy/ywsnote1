Maven 构建工具（把项目从头-编译-打包-部署）
Hadoop是在分布式服务器集群上存储海量数据并运行分布式分析应用的一个平台，其核心部件是HDFS与MapReduce
HDFS是一个分布式文件系统：传统文件系统的硬盘寻址慢，通过引入存放文件信息的服务器Namenode和实际存放数据的服务器Datanode进行串接。对数据系统进行分布式储存读取。
MapReduce是一个计算框架/编程模型：MapReduce的核心思想是把计算任务分配给集群内的服务器里执行。通过对计算任务的拆分（Map计算\Reduce计算）再根据任务调度器（JobTracker）对任务进行分布式计算。大规模并行运算
Map映射
Reduce归约
YARN（资源管理系统）
HBase（分布式、多版本、无模式、稀疏、面向列的开源数据库），
利用hadoop HDFS作为文件存储系统，
利用Zookeeper作为协同服务，
利用MapReduce处理HBase中的海量
利用Hive挖掘HBase中的海量，将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行
利用Spark分析HBase中的海量
Mongodb 文档型NoSql
Redis Key-Value型NoSql，缓存型。
ElasticSearch是一个基于Lucene的搜索服务器。有分布式多用户能力的全文搜索引擎，基于RESTful web接口。用Java开发的，设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便，es可以解决在数据量大又要求实时性高的问题。
 
Scala 支持函数式和面向对象编程的语言，运行在JVM上的，简洁的静态语言。
Kafka 分布式流式处理平台，软交换，缓冲作用。
