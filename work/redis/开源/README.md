redis.io 官方支持的部署模式有 4 种:

- 单机/Standalone：开发、测试、小流量
- 主从复制/Replication：读扩展、数据备份（只用于单分片，3个节点1主2从）
- Sentinel/High Availability：主从 + 自动故障转移（单分片3个节点1主2从，再加3个哨兵-用于主挂后重新选主）
- Cluster/Sharding + HA：多分片（内置高可用，所以不再需要额外的）


持久化方式：

RDB（redis database/快照/二进制）+AOF（Append Only File/增量log/RESP文本）




## 不同于mysql（磁盘型数据库，持久化是根本，内存只是缓存）
（1）Buffer Pool：mysql-InnoDB 在内存里维护的一个大缓存
所有读写都先操作 buffer pool，不是直接读写磁盘，修改过的页叫 脏页（dirty page），后台线程异步刷回磁盘（.ibd 文件）
（2）redo log（类似 Redis 的 AOF），在Buffer Pool没刷盘之前保留的用于崩溃恢复；
这个机制也叫WAL（Write-Ahead Logging）原则：日志先于数据落盘；
（3） undo log（回滚日志）：用于事务相关，MVCC版本链，让快照读能读到历史版本；
（4）binlog（二进制日志）—— MySQL Server 层的日志，而不是 InnoDB 的，用于主从复制；
