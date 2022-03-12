# RDD dataframe dataset：全都是spark平台下的*分布式弹性数据集*，为处理超大型数据提供便利

RDD（Resilient Distributed Dataset）是分布式的Java对象的集合。DataFrame是分布式的Row对象的集合
DataFrame相比RDD提供了详细的结构信息，使得Spark SQL可以清楚地知道该数据集中包含哪些列，每列的名称和(类型?)各是什么。即schema
DataFrame除了提供了比RDD更丰富的算子以外，更重要的特点是提升执行效率、减少数据读取以及执行计划的优化，比如filter下推、裁剪等
Dataframe的劣势在于在编译期缺少类型安全检查，导致运行时出错
dataset是Dataframe API的一个扩展，是Spark最新的数据抽象；用户友好，既具有类型安全检查也具有Dataframe的查询优化特性；支持编解码器，当需要访问非堆上的数据时可以避免反序列化整个对象，提高了效率
Dataframe是Dataset的特列，DataFrame=Dataset[<type>] ，所以可以通过as方法将Dataframe转换为Dataset
三者都有惰性机制，在进行创建、转换，如map方法时，不会立即执行，只有在遇到Action如foreach时，三者才会开始遍历运算，极端情况下，如果代码里面有创建、转换，但是后面没有在Action中使用对应的结果，在执行时会被直接跳过
