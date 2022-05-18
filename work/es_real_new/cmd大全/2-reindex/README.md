## 通过reindex+别名进行索引切换的方法
```
（创新的索引名），（把实时索引别名指向新的），（reindex），（把查询别名指向新的），（删旧索引名）

PUT <index_name>
PUT <index_name>/_alias/<index_alias_name>
DELETE <old index_name>/_alias/<index_alias_name>

POST _reindex
{
  "source": {
    "index": <old index_name>"
  },
  "dest": {
    "index": "<index_name>"
  }
}

PUT <index_name>/_alias/<search_alias_name>
DELETE <old index_name>/_alias/<search_alias_name>
DELETE <old index_name>
```