## from+size
越往后，效率越低，并且不能保证实时变化带来的页之间的重复

## search_after
实时变化可以生效，不能随机跳页，依赖上一次的sort位置，必须有sort条件，并且sort字段的值能保证唯一

## scroll
会生成快照，实时变化不会体现在翻页中，不适于实时查询

- 1，先发命令，获取第一页结果及scroll id
```
GET <index_name>/<type>/_search?scroll=10m
{
  "from": 0,
  "size": 1,
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "_id": "asc"
    }
  ]
}
```
- 2. 后面每一页只需要用第一次返回的scroll id查询即可，直到返回的hit为空即代表查询结束
```
GET _search/scroll
{
  "scroll_id": "xxxx",
  "scroll": "10m"
}
```