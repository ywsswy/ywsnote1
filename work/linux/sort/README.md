cat file |sort -t ':' -k3
# 文件按:分割后的第3列(1s)升序排列
# -n 转换成数字
# -r 降序//默认是升序

count某一列，sort后|uniq -c|sort

uniq就把数量统计出来了，并输出到第一列