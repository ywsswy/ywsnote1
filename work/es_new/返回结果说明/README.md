
took - Elasticsearch执行搜索的时间（以毫秒为单位）
timed_out - 告诉我们搜索是否超时
_shards - 告诉我们搜索了多少个分片，以及搜索成功/失败分片的数量
hits - 搜索结果
hits.total - 符合我们搜索条件的文档总数
hits.hits - 实际的搜索结果数组（默认为前10个文档）
hits.hits.sort - 对结果进行排序键（如果按分数排序则丢失）