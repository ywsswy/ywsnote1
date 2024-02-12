
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