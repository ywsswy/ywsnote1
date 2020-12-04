# rsync -vazu --progress --delete <A> <B>
|A机|B机|效果|
|---|---|---|
|xxx|yyy|A机器xxx文件夹 同步到B机器yyy/xxx文件夹|
|xxx/|yyy|A机器xxx文件夹 同步到B机器yyy文件夹(B机没有yyy会创建的)|


## 综上两个目录之间的命令一般应该
rsync -vazu --progress --delete <folder>/ <ip::path>/<folder>

## 本地代码和服务同步（只需要在本地操作，要【WSL】 /root盘里）
rsync -vazu --progress --delete <remote/proj> . # 远程目录拷贝到本地
cd proj && do something 注意只能在wsl中修改文件，不能在windows文件管理器的搞（增删肯定不行，修改还是可以的……
rsync -vazu --progress --delete <remote/proj>/ . # 远程改变、同步到本地
rsync -vazu --progress --delete ./ <remote/proj> # 本地改变、同步到远程

# 最牛的，查看远程服务器目录
rsync <remote/proj>/

# 单独删除一个文件（注意当前目录不要存在这个文件）
rsync -vd --filter="R 1.txt" --filter="P *" --delete-excluded --existing --ignore-existing . user@address:path

# 指定端口
-e 'ssh -p 18822'
