2023-03-06 06:13:23,919 INFO [Thread-55] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: mapResourceRequest:<memory:2048, vCores:1>
2023-03-06 06:13:23,923 INFO [AsyncDispatcher event handler] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: Size of event-queue in RMContainerAllocator is 1000
2023-03-06 06:13:23,926 INFO [AsyncDispatcher event handler] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: Size of event-queue in RMContainerAllocator is 2000
2023-03-06 06:13:23,926 INFO [AsyncDispatcher event handler] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: Size of event-queue in RMContainerAllocator is 2000
2023-03-06 06:13:23,926 INFO [AsyncDispatcher event handler] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: Size of event-queue in RMContainerAllocator is 2000
2023-03-06 06:13:23,928 INFO [AsyncDispatcher event handler] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: Size of event-queue in RMContainerAllocator is 3000
2023-03-06 06:13:23,953 INFO [Thread-55] org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator: reduceResourceRequest:<memory:4096, vCores:1>


这里可以看到map和reduce的内存设置

不合理可以调整参数，例如：
set mapreduce.reduce.java.opts=-Xmx6144M;