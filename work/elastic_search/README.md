https://blog.csdn.net/jiaojiao521765146514/article/details/83750548


查询所有索引信息
curl -X GET "$dev_host/_cat/indices?v&pretty"


从json文件插入文档（可以指定id）
curl -H "Content-Type: application/json" -XPOST "$dev_host/$index/$type/_bulk?pretty&refresh" --data-binary @accounts.json
