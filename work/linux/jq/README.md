# 参数
--sort-keys ## 按照key排序
-c #一行输出，而不是格式化输出
-r # 输出字符串时不带双引号

# 命令
|length

|keys


# 语法

对于object的访问
.<key>
如果key本身还有.符号，可以用"<key>"来实现

同时输出两个key（每个一行）
.<key1>,.<key2>

输出成一个数组对象
[.<key1>,.<key2>]

输出成空格分隔的一行，还有其他格式，例如@json @csv
[.<key1>,.<key2>] |@sh

对于数组的访问
[0]

输出对象数组中的每一个对象的某字段值
方法一
[].<key>
方法二（推荐因为可以同时输出key1 key2）
[] | .<key>


{mykey: <过滤器>}

curl -X GET -s "$ipaddr/$index/$type/_search?pretty" -H 'Content-Type: application/json' -d "" |jq '.hits.hits[0]._source' --sort-keys