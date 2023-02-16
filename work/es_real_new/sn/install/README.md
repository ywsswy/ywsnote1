https://www.elastic.co/elastic-stack/(官方提供了yum仓库的方式安装, systemctl启动)


配置文件在/etc/elasticsearch/elasticsearch.yml
日志在/var/log/elasticsearch
系统启动(systemctl)文件在/usr/lib/systemd/system/elasticsearch.service # 这里表明启动的User & Group都是elasticsearch

kibana类似
/etc/kibana/kibana.yml
server.port: 5601
server.host: "localhost" # 不允许远程连接，否则改成"0.0.0.0"
server.basePath: "/kibana" # 一般不对外暴露5601端口，用nginx转发，同时，也不能霸占nginx的一个端口，所以配置反向代理到某location
server.rewriteBasePath: true