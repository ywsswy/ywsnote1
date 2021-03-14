（创新），（reindex），（写别名），（删旧别名），（删旧）


POST _reindex
{
  "source": {
    "index": "old_index"
  },
  "dest": {
    "index": "new_index"
  }
}


PUT <index>/_alias/<index_alias_name>

DELETE <index_name>/_alias/<index_alias_name>