https://zhuanlan.zhihu.com/p/383999276


lucene支持两种存储：Stored fields 和 doc values

es可以在mapping中配置指定某个字段是否启用 Stored Fields 和 Doc Values
默认情况下,store为false;而除了text类型的字段外,doc_values都为true.

- Stored fields在磁盘上以行的方式放置: 每个文档对应一个行,这个行包含该文档所有的stored fields.
- source是一个大文档json，es把source 字段放在了stored fields中的第一个字段；（es查询时使用_source语句）
- Doc values 以列的方式存储. 可以直接访问一个特定的文档的特定字段（计算想要字段值的偏移地址）；（es查询时使用docvalue_fields语句）