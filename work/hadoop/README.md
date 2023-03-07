web UI上，应用程序（Application）页面，
Application Type: 可以查看类型MAPREDUCE/SPARK
Tracking URL：可以查看详细信息，如果是spark程序，看到的是jobs视图，mapred程序，看到的是在Map任务和Reduce任务进度；

## 其他
- YARN（Yet Another Resource Negotiator）是Hadoop 2.x版本引入的一种资源管理和调度系统，用于管理和调度集群中的资源，支持多种计算框架（如MapReduce、Spark、Flink等）运行在同一个Hadoop集群中，提高了集群的资源利用率和灵活性；YARN由两个主要的组件构成：ResourceManager和NodeManager。
1）ResourceManager：负责整个集群的资源管理和调度（生命周期与集群的生命周期一致）。ResourceManager启动时会为每个应用程序分配一个ApplicationMaster（生命周期只在一个程序运行时）。
2）NodeManager：负责单个节点的资源管理和任务执行。NodeManager在每个节点上启动，负责为运行在该节点上的应用程序提供计算资源

- Spark
处理逻辑上，每个Job都会在RDD上执行一种行动(Action)，比如count()或saveAsTextFile()等，
相互关系上，一个Spark应用程序可能包含多个Job，一个Job包含多个Stage，每个Stage包含多个Task，通常每个Task是在一个Executor上执行，通常对应一个分区（Partition）的数据；

- MapReduce
在MapReduce中，当Map任务生成中间结果并传递给Reduce任务时，Reduce任务需要将中间结果进行排序和合并。在进行排序和合并时，会将中间结果分成多个小文件，这些小文件被称为“segments”，每个segment都包含一定数量的中间结果键值对。