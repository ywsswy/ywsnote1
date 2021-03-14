PUT <new index name>  # 基本上从旧的上面copy一下就能设置到新的上来（变化是，少了一层，删了aliases，settings.index.provided_name|creation_date|uuid|version
{
  "mappings": {...},
  "settings": {...}
}


查询

GET <index name>