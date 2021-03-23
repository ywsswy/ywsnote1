两阶段提交（2PC）绝对是CP的死党，是分布式情况下强一致性算法，因此缺点也是很明显的，

单点coordinator是个严重问题：

没有热备机制，coordinator节点crash了或者连接它的网路坏了会阻塞该事务；

吞吐量不行，没有充分发动数量更多的participants的力量，一旦某个participant第一阶段投了赞成票就得在他上面加独占锁，其他事务不得介入，直到当前事务提交or回滚


phapaxos怎么处理的