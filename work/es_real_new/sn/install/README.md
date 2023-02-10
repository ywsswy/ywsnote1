https://www.elastic.co/elastic-stack/(官方提供了yum仓库的方式安装, systemctl启动)


配置文件在/etc/elasticsearch/elasticsearch.yml
日志在/var/log/elasticsearch

kibana类似
/etc/kibana/kibana.yml
server.port: 5601
server.host:  "localhost" # 不允许远程连接，否则改成"0.0.0.0"


配置nginx转发
https://blog.csdn.net/a80C51/article/details/108275934