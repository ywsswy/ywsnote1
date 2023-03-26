小的不用分片，shards为1
副本数replicas为2

建表索引
/mappings/properties{每个字段名:{index:是否会被查询,type:类型}}



"type": "keyword"表示不分词
        "text" 不指定分词器，就会分词

"index": false,表示不会用这个字段来查询,也就等价于不进行分词


"doc_values": false, 不存正派索引

"search_analyzer": "jieba_index",
"analyzer": "jieba_search",


如果不指定的话，keyword默认是index为true

# 创建索引index 
curl -XPUT "http://$ipaddr/_template/$index?pretty" -H 'Content-Type:application/json' -d @template.json

# 删除索引curl -XDELETE "http://$ipaddr/$index_name?pretty"


# 查询有哪些index
curl -X GET "$dev_host/_cat/indices?v&pretty" |grep 

# 查询某个index的信息
curl -XGET "http://$ipaddr/$index" |jq .


# 设置某个索引Index的别名
curl -s -XPOST -H 'Content-Type: application/json' 'http://$ipaddr/_aliases' -d'
{
  "actions":[
     {"add" : {"index": "<INDEX_NAME>", "alias": "<INDEX_ALIAS>"}}
  ]
}'