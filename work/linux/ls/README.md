ll |sed -n "5p" |awk '{print $9}'  #获取第5行第9列的内容
ls |grep -v da_h.cmake |grep -v CMakeLists.txt | xargs rm -rf # 删除某两个文件以外的文件
这种危险的命令一定要去到当前目录执行
ll --time-style=long-iso 显示详细时间

-r 倒序
-S 文件大小排序
-h 可读性显示
-t 时间排序