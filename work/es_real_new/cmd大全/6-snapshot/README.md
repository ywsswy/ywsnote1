- 兼容性要求：（1）恢复/restore时es版本不得低于生成快照的版本；（2）版本对照要求：https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshot-restore.html#how-snapshots-work


参考https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-filesystem-repository.html
1）修改配置文件，设置快照存储路径
path.repo: /root/<x>

不过因为es启动用户不是root，所以要（1）给es的默认用户加到root组里，usermod -a -G root elasticsearch（2）开放文件夹（及其所有祖先目录）给本组内可读写chmod g+w <x>

2）
```
PUT _snapshot/<repository>
{
  "type": "fs",
  "settings": {
    "location": "<y>"
  }
}
```
3） 查询有哪些快照仓库
GET _snapshot
4）往快照里生成快照（wait_for_completion表示不要放到后台去执行）
```
PUT _snapshot/<repository>/<snapshot_name>?wait_for_completion=true
{
  "indices": "<index>"
}
```
5）把快照文件夹(/root/<x>/<y>)保存起来
6）新机器上，解压快照文件夹，重复1～2
7）可以查看仓库/磁盘里有哪些快照有效
GET _snapshot/<repository>/*?verbose=false
8）恢复/restore快照
```
POST _snapshot/<repository>/<snapshot_name>/_restore?wait_for_completion=true
{
  "indices": "<index>"
}
```

## 其他
- 删除某快照仓库
DELETE _snapshot/<repository>

- 删除某快照
DELETE _snapshot/<repository>/<snapshot_name>