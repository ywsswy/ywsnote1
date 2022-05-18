https://blog.csdn.net/jiaojiao521765146514/article/details/83750548
dev_host=localhost:9200
index1=customer
index2=here2
index3=bank
type=test


查询所有索引信息
curl -X GET "$dev_host/_cat/indices?v&pretty"

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
