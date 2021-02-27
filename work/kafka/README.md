producer:允许应用程序发布一个消息至一个或多个kafka的topic中

consumer:允许应用程序订阅一个或多个主题，并处理所产生的对他们记录的数据流

topic/主题: 消息以topic为类别记录,为了提高吞吐量，每个消息主题又会有多个分区partition

broker/一个服务节点：以集群的方式运行,消费者从Broker拉数据

record记录/消息：是由一个key，一个value和时间戳构成

partition/分区：都是一个顺序的、不可变的消息队列,并且可以持续的添加，每个partition在存储层面是append log文件。序列号/偏移量(offset),在每个分区中此偏移量都是唯一的

Kafka集群保持所有的消息,直到它们过期,无论消息是否被消费了
实际偏移量由消费者控制，消费者可以将偏移量重置为更老的一个偏移量，重新读取消息

一个Topic的多个partitions,被分布在kafka集群中的多个server上;
此外kafka还可以配置partitions需要备份的个数(replicas),每个partition将会被备份到多台机器上,以提高可用性
就是说以partitons为粒度的 分行分列都是存在的，不用我来感知

同一个消费组/consumer group内的所有消费者（多个进程/线程）协调在一起（具有容错性的消费者机制）来消费订阅主题(subscribed topics)的所有分区(partition)。
当然，每个分区只能由同一个消费组内的一个consumer来消费。
消费组存在的意义就是可以保证同一个消费组里面多个消费示例消费消息。再来另一个消费组可以互不干扰。
一个消费模式概念上的说法是group内是load balancing，group间是fan-out
- load balancing（负载均衡）：共享订阅、提高性能
- fan-out（扇出）：各自订阅、互不影响