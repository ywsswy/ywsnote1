应用程序（Application）： 基于Spark的用户程序，包含了一个Driver Program 和集群中多个的Executor；

驱动程序（Driver Program）：运行Application的main()函数并且创建SparkContext，通常用SparkContext代表Driver Program；

执行单元（Executor）： 是为某Application运行在Worker Node上的一个进程，该进程负责运行Task，并且负责将数据存在内存或者磁盘上，每个Application都有各自独立的Executors；

有向无环图（DAG）：Directed Acycle graph，反应RDD之间的依赖关系；

窄依赖（Narrow dependency）：子RDD依赖于父RDD中固定的data partition；

宽依赖（Wide Dependency）：子RDD对父RDD中的所有data partition都有依赖

reduceByKey会将一个RDD中的每一个key对应的所有value聚合成一个value，然后生成一个新的RDD，元素类型是<key,value>对的形式，这样每一个key对应一个聚合起来的value。

每一个key对应的value不一定都是在一个partition中，也不太可能在同一个节点上，因为RDD是分布式的弹性的数据集，它的partition极有可能分布在各个节点上。

聚合形式：(shuffle就是链接map和reduce的桥梁过程)
Shuffle Write：上一个stage的每一个map task就必须保证将自己处理的当前分区中的数据相同的key写入一个分区文件中，可能会写入多个不同的分区文件中。
Shuffle Read：reduce task就会从上一个stage的所有task所在的机器上寻找属于自己的那些分区文件，这样就可以保证每一个key所对应的value都会汇聚在同一个节点上去处理和聚合。