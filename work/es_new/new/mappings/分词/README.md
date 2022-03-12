tokenizers（分词器）, token-filter（分词过滤器）以及 analyzers（分析器）
其中分词器作用是哪里切一刀
过滤器作用是切出来的怎么转换，例如大写转小写
分析器是分词器和过滤器的组合

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



## 使用特定的分词器分析一个query/doc的分词结果
POST <index_name>/_analyze
{
  "analyzer": "ik_max_word", 
  "text": "爱很美味"
}

## 几种分词
- n-gram
我爱你 => 我 爱 你 我爱 爱你 我爱你
- 按字符长度从1到n进行切割的前缀分词
我爱你 => 我 我爱 我爱你