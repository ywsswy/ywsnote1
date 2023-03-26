1) settting to only sync the bookmarks.

2) [check if bookmarks already synced.](chrome://sync)

Local State:
Server Connection	OK since 2019年5月21日星期二 上午1:27:49
Last Synced	Just now
Sync First-Time Setup Complete	true
Sync Cycle Ongoing	false
Local Sync Backend Enabled	false
Local Backend Path	Uninitialized

# 重点是看中间的日志信息
作为发起修改方，保证所有的commit request都得到了SYNCER_OK的commit response日志。
作为接受修改方，保证在修改之后（看time）得到了GetUpdates Response日志。
