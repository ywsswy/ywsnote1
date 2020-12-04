单机版搭建
https://www.jianshu.com/p/d8a21ab2c2d3


0）mq控制台 $web_ipaddr/?nameSrvAddress=$name_server_ipaddr%3A9876#/topic
1）启动namaserver （默认是9876端口）
nohup sh bin/mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log

2）启动broker
nohup sh bin/mqbroker -n localhost:9876 &
tail -f ~/logs/rocketmqlogs/broker.log


3）使用example收发消息
export NAMESRV_ADDR=localhost:9876

sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer

sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer