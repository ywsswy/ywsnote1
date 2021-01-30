名字服务：集群动态管理/健康检查 负载均衡



|-|一致性算法|接口|优点|缺点|ps|
|---|---|---|---|---|---|
|zookeeper|paxos|sdk|dubbo框架支持<br>提供watcher机制能实时获取服务提供者的状态|<font color=red>没有健康检查，不支持多数据中心|跨机房部署的时候，zookeeper也应该每个机房单独部署各自有一个master，防止跨机房还只有一个master|
|consul|raft|http/dns|有健康检查，支持多数据中心|不能实时获取服务信息的变化通知（不支持sub/pub订阅机制，所以服务发现，得使用者自己去HTTP轮训发现变更）|
|etcd|raft|http|-|没有健康检查，不支持多数据中心|
|l5|



当zookeeper的master挂了时，需要重新选举，此时服务是不可用的。
CAP 一致性（Consistency）、可用性（Availability）、分区容错性（Partition tolerance）
一种说法：服务注册中心应该是个AP系统，因为一台机器上的C短暂的没了造成短暂的负载均衡或者服务发现不够稳定，不至于完全无法服务；但是zookeeper是个CP系统。。

Q：

zookeeper没有健康检查？？？
