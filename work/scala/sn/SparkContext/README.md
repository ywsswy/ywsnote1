
SparkContext是Spark中Driver程序的一部分，向资源管理器cluster manager（可以是mesos、yarn、standalone）申请spark应用所需的资源executor，资源管理器在各个worker上分配一定的executor
用于创建和操作RDD
SparkSession，实质上是SQLContext、HiveContext、SparkContext的组合
在2.x之前，对于不同的功能，需要使用不同的Context——
创建和操作RDD时，使用SparkContext
使用Streaming时，使用StreamingContext
使用SQL时，使用sqlContext
使用Hive时，使用HiveContext
在2.x中，为了统一上述的Context，引入SparkSession