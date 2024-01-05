https://blog.csdn.net/qq_29116427/article/details/80206125


- 生产
注意input.data（每行是key\tmsg）中消息键与消息值间使用“Tab键”进行分隔，消息值里面切勿使用转义字符(\t)
cat input.data | ./kafka_2.11-1.1.1/bin/kafka-console-producer.sh --broker-list <ip:port> --topic <topic> --property parse.key=true

- 消费

./kafka_2.11-1.1.1/bin/kafka-console-consumer.sh --bootstrap-server <ip:port> --topic <topic> --property print.key=true --property print.timestamp=true

--from-beginning 从最开始消费，否则是从latest消费

--partition <p_id(0s)> 指定分区，否则全部分区

--offset <位点> 指定，否则是从latest消费