https://blog.csdn.net/jiaojiao521765146514/article/details/83750548
dev_host=localhost:9200
index1=customer
index2=here2
index3=bank
type=test

查看版本信息返回的/version/number即是版本信息
curl -X GET "$dev_host"

查询所有索引信息
curl -X GET "$dev_host/_cat/indices?v&pretty"

新建索引同时指定分片、type、字段 默认5分片shards，1副本replicas
curl -XPUT "$dev_host/$index/?pretty" -H "Content-Type: application/json" -d ' 
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  },
  "mappings": {
    "'$type'": {
      "properties": {
        "name": {
          "type": "text"
        },
        "area": {
          "type": "text"
        }
      }
    }
  }
}
'

查看mapping字段信息类型
curl -XGET "$ipaddr/$index/_mapping"

删除索引
curl -X DELETE "$dev_host/$index?pretty&pretty"

插入文档
curl -X PUT "$dev_host/$index/$type/$id" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}
'
查询文档
curl -X GET "$dev_host/$index/$type/$id?pretty&pretty"

更新文档
curl -X POST "$ipaddr/$index/$type/$id/_update?pretty&pretty" -H 'Content-Type: application/json' -d'
{
  "doc": { "name": "Jane Doe" }
}
'
删除文档
curl -X DELETE "$dev_host/$index/$type/$id?pretty&pretty"

从json文件插入文档（可以指定id）
curl -H "Content-Type: application/json" -XPOST "$dev_host/$index/$type/_bulk?pretty&refresh" --data-binary @accounts.json

查找数据
curl -X GET -s "$ipaddr/$index/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match_all": {} }
}
'
curl -X GET "$dev_host/$index/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "name": "banana" } }
}
'
明确字段是否需要分词，不需要分词的字段就将type设置为keyword，
Standard Analyzer 默认分词器，按词切分，已小写处理（中文会切成单字）

查看某个字段分词的结果
curl -X POST "$dev_host/$index/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "field": "name",
  "text": "东北 赵四"
}
'
在建立索引时，只会去看字段有没有定义analyzer，有定义的话就用定义的，没定义就用ES预设的
在查询时，会先去看字段有没有定义search_analyzer，如果没有定义，就去看有没有analyzer，再没有定义，才会去使用ES预设的
index_analyzer
Index表示该字段是否索引，如果index为no那个analyzer设为啥也没用
"index":"not_analyzed"