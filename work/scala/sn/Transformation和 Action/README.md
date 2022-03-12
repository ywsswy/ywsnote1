一个完整的RDD任务由两部分组成（两种计算方式）：Transformation和 Action

（1） transformation/转换：得到一个新的RDD，比如从数据源生成一个新的RDD，从RDD生成一个新的RDD
transformation操作不会触发spark程序执行的，它们只是先记录了对RDD所做的一系列操作，只有之后碰上一个action操作，那么前 面所有的transformation才会执行
常见：数据之间的聚合运算（reduceByKey），排列组合(sortByKey)
包括map，filter，groupBy，join等

（2）action/操作：action是得到一个值，或者一个结果（直接将RDD cache到内存中）
包括：count，reduce，collect

action  会触发  job 操作
transformation 是 lazy 模式，在计算action之前不会执行