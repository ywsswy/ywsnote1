tokenizers（分词器）, token-filter（分词过滤器）以及 analyzers（分析器）
其中分词器作用是哪里切一刀
过滤器作用是切出来的怎么转换，例如大写转小写
分析器是分词器和过滤器的组合

分词器必须配置在索引下，创建时指定在settting里，然后mapping里字段就可以配置使用
- setting示例
{  
   "settings":{  
      "analysis":{  
         "analyzer":{  
            "my_custom_analyzer":{  //这里自定义了一个带有停用词和同义词的分析器
               "type":"custom",
               "tokenizer":"standard",
               "filter":[  
                  "lowercase",
                  "english_stop", //使用自定义的过滤器
                  "synonyms"
               ]
            }// 可以继续 ,{...} 自定义下一个分析器
         },
         "filter":{  
            "english_stop":{  //自定义的过滤器
               "type":"stop",
               "stopwords":"_english_"
            },
            "synonym":{  
               "type":"synonym",
               "synonyms":[  
                  "i-pod, ipod",
                  "universe, cosmos"
               ]
            }
         }
      }
   }
}
- mapping示例
{
  "xxx" : {
    "mappings" : {
      "doc" : {
        "properties" : {
          "alias" : { //字段，查询可以用 "query": {"term": {"alias": value 来查询
            "type" : "text",
            "similarity" : "title_similarity",
            "fields" : { //这个表示alias字段还有属性
              "keyword" : { //属性1:keyword，查询可以用"query": {"term": {"alias.keyword": value 来查询
                "type" : "text",
                "analyzer" : "lowercase_keyword"
              }
            },
            "analyzer" : "lowercase_ik_max_word" //这个是alias的分析器
          },

mapping里明确字段是否需要分词，不需要分词的字段就将type设置为keyword，
在建立索引时，只会去看字段有没有定义analyzer，有定义的话就用定义的，没定义就用ES预设的
在查询时，会先去看字段有没有定义search_analyzer，如果没有定义，就去看有没有analyzer，再没有定义，才会去使用ES预设的
index_analyzer
Index表示该字段是否索引，如果index为no那个analyzer设为啥也没用
"index":"not_analyzed"


## 几种分词
- n-gram
我爱你 => 我 爱 你 我爱 爱你 我爱你
- 按字符长度从1到n进行切割的前缀分词
我爱你 => 我 我爱 我爱你
- Standard Analyzer
默认分词器，按词切分，已小写处理（中文会切成单字）
