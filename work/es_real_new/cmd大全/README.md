## 这里列举所有cmd，但是并不展开讲解
具体需要往下查看


### 调试分词结果
curl -XPOST '$ipaddr/_analyze?pretty' -H 'Content-Type:application/json' -d'{ "analyzer": "jieba_index", "text": "小吃快餐" }'

### 单个文档信息查询
GET <index>/_doc/<id>

### DSL 查询
GET <index>/_search
{
  "explain": true,
  "_source": {
    "includes": [
      "title"
    ]
  },
  "query": {
    "match_all": {}
  },
  "size": 10
}

### [1]查询索引的alias、mapping、setting
GET <index name>

### [2]reindex & alias修改
POST _reindex
{
  "source": {
    "index": "old_index"
  },
  "dest": {
    "index": "new_index"
  }
}

### [3]使用某索引里面的特定的分析器分析一个query/doc的分词分析结果
POST <index_name>/_analyze
{
  "analyzer": "ik_max_word", 
  "text": "爱很美味"
}

### 查看es版本（/version.number即是）
GET /

### [4] 查看es磁盘保护：read_only_allow_delete

### [5] 查询模板
查询：
curl -X GET "localhost:9200/_search/template" -H 'Content-Type: application/json' -d'
{
    "id": "<templateName>",
    "params": {
        "query_string": "search for these words"
    },
    "explain": false//true的话会开启得分说明
}
'

### [7] scroll 翻页
