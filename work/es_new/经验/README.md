部署的时候分配多大的数据节点，建议是一个节点容量*95%能够撑得下所有index（并且最大的index要乘以2，以便后续reindex）
- 磁盘到达95%时会触发只读机制（可以通过GET _cluster/settings判断）（后果甚至会导致kibana监控看不到了）
- 只读的恢复方法是：

```
PUT _settings
{
  "index": {
    "blocks": {
      "read_only_allow_delete": "false"
    }
  }
}
```

- kibana监控的原理是，读取.monitoring-es-6-<yyyy.mm.dd>的索引库，通过查询可以看出来存了哪些监控（注意时区）
```
GET .monitoring-es-6-2021.12.01/_search
{
  "size": 0,
  "query": {
    "term": {
      "type": "cluster_stats"
    }
  },
  "aggs": {
    "group_by_day": {
      "date_histogram": {
        "field": "timestamp",
        "interval": "hour"
      }
    }
  }
}
```

cpu超过70%有风险
