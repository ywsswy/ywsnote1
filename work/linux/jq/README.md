# 参数
--sort-keys ## 按照key排序
-c #一行输出，而不是格式化输出


# 语法

对于object的访问
.<key>

对于数组的访问
[0]

{mykey: <过滤器>}

curl -X GET -s "$ipaddr/$index/$type/_search?pretty" -H 'Content-Type: application/json' -d "" |jq '.hits.hits[0]._source' --sort-keys