概念：
小的不用分片，shards为1
副本数replicas为2（加上1个primary表示同一份数据有三个节点上都有）

创建索引、mapping、setting
PUT <new index name>  # 基本上从旧的上面copy一下就能设置到新的上来（变化是，少了一层，删了aliases，settings.index.provided_name|creation_date|uuid|version
{
  "mappings": {...},
  "settings": {...}
}



删除索引
DELETE /<index name>