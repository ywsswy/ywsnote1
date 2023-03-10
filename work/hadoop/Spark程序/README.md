处理逻辑上，每个Job都会在RDD上执行一种行动(Action)，比如count()或saveAsTextFile()等，
相互关系上，一个Spark应用程序可能包含多个Job，一个Job包含多个Stage，每个Stage包含多个Task，通常每个Task是在一个Executor上执行，通常对应一个分区（Partition）的数据；
