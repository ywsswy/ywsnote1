设置：

curl -X POST "localhost:9200/_scripts/testtemplate" -H 'Content-Type: application/json' -d'
{
    "script": {
        "lang": "mustache",
        "source": """
        {
            "query": {
                "match": {
                    "title": "{{query_string}}"
                }
            }
        }
"""
    }
}
'

查看：
curl -X GET "localhost:9200/_scripts/testtemplate"

渲染测试：
curl -X POST "localhost:9200/_render/template/twittertemplate" -H 'Content-Type: application/json' -d'
{
	"params": {
		"query_string": "kimchy2"
	}
}
'

删除：
curl -X DELETE "localhost:9200/_scripts/<templatename>"
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