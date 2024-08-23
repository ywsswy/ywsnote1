head -c <num> 可以只看前几个字符

head -n 3 <file>  # 显示文件前3行
head -n -3 <file>  # 显示文件从前面开始显示，最后3行不显示

tail -n +4 <file>  # 从第4行开始显示，一直到文件结束