1) 检索单条信息
HTTP GET请求并指出文档的“地址”(/<索引>/<类型>/<ID>)既可。根据这三部分信息，我们就可以返回原始JSON文档
GET /megacorp/employee/1

2) 返回前10条(/<索引>/<类型>/_search)
GET /megacorp/employee/_search
响应的hits中的hits包含了所有信息

3) 查询last_name字段值为Smith的
GET /megacorp/employee/_search?q=last_name:Smith
或者用DSL查询，get带上json请求体
GET /megacorp/employee/_search
{
    "query" : {//除了query还有过滤器filter
        "match" : {//不是完全相等，是相关即可，最牛逼的地方，//除了match还有"match_phrase"，必须确切包含某些词
            "last_name" : "Smith"
        }
    }
}