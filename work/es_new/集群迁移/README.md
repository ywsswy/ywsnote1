npm install elasticdump -g

elasticdump --input=http://xxx/<index> --httpAuthFile httpAuthFile --output=http://xxx/<index> --type=settings
elasticdump --input=http://xxx/<index> --httpAuthFile httpAuthFile --output=http://xxx/<index> --type=mapping
elasticdump --input=http://xxx/<index> --httpAuthFile httpAuthFile --output=http://xxx/<index> --type=alias  # output可以是文件

查询模板要自己插入，先看源
GET _cluster/state/metadata?pretty&filter_path=**.stored_scripts
然后逐条插入


- 相关概念：
关闭负载均衡
reindex

是否可以停止服务、停止写入

mapping 6和7不兼容
https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html#_typeless_apis_in_7_0