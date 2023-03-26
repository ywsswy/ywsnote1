* 什么时候用get 什么时候用post

* 提前指定了模板后，怎么灌数据

* 未指定模板的时候直接灌数据的mapping自动是这个
curl -XGET "$dev_host/$index/_mapping?pretty"
{
  "customer" : {
    "mappings" : {
      "test" : {
        "properties" : {
          "account_number" : {
            "type" : "long"
          },
          "balance" : {
            "type" : "long"
          },
          "name" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          }
        }
      }
    }
  }
}