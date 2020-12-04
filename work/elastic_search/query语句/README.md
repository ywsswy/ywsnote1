curl -X GET "$dev_host/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "address": "mill lane" } }
}
'

match 指定特定字段的查询，内的单词是Or的关系mill 或 lane

match_phrase 内是一个短语，整个匹配，中间的空格可以多个，（竟然没有区分大小写）

bool must子句
bool should子句
"query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }

同层之间逗号分隔
bool {
--/must [
--/must_not [
--/should [ {..},{..}
--/filter { //filter的作用是把那些无需计算匹配度的字段放进来
--/--/range {