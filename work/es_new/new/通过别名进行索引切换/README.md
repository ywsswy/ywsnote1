（创新的索引名），（把实时索引别名指向新的），（reindex），（把查询别名指向新的），（删旧索引名）


POST _reindex
{
  "source": {
    "index": "old_index"
  },
  "dest": {
    "index": "new_index"
  }
}


PUT <index_name>/_alias/<index_alias_name>

DELETE <index_name>/_alias/<index_alias_name>

DELETE <index_name>