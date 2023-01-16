hadoop fs -<command>

支持的命令
hadoop fs -put localdir remotedir #上传文件
ls
mkdir
cat
hadoop fs -put -f 本地文件 hdfs文件 #覆盖文件
hadoop fs -get hdfs文件 本地文件
hadoop fs -rm -r


mv 移动文件（源文件不能存在）

不支持的命令
tree
