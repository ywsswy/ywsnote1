删除有两种，一种是删除索引库，另一种是删除索引库里的文档

1、删除索引库最危险，而且如果有文档写入的时刻，可能删不成功
DELETE <index>


2、删除索引库里的文档也很危险，最好事先有备份（比如reindex）
POST <index>/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}