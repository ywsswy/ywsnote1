## 如果是备份，建议使用es自带的快照功能（官方建议）

https://blog.51cto.com/u_15316348/3217704?b=totalstatistic
npm install elasticdump -g

elasticdump --input=http://xxx/<index> --httpAuthFile httpAuthFile --output=http://xxx/<index> --type=settings
elasticdump --input=http://xxx/<index> --httpAuthFile httpAuthFile --output=http://xxx/<index> --type=mapping
elasticdump --input=http://xxx/<index> --httpAuthFile httpAuthFile --output=http://xxx/<index> --type=alias  # input和output可以是本地文件
这个工具也有--type=data的数据迁移，但是不确定好不好用。。

查询模板要自己插入，先看源
GET _cluster/state/metadata?pretty&filter_path=**.stored_scripts
然后逐条插入


- 相关概念：


mapping 6和7不兼容
https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html#_typeless_apis_in_7_0

httpAuthFile格式：
`user=<username>
password=<password>`


## 集群缩容问题：
- max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
把/etc/sysctl.conf中的加上配置项，赋值262144
- 内存不够：es的配置yml中的min和max设置不能超过50%
- kibana失败：Elasticsearch is still initializing the kibana index
把es中的kibana数据删掉：curl -XDELETE http://localhost:9200/.kibana
curl -s "http://127.0.0.1:9200/_cat/indices" # 可以看到有哪些索引
