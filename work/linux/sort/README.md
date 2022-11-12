cat file |sort -t ':' -k3
# 文件按:分割后的第3列(1s)升序排列
# -n 转换成数字
# -r 降序//默认是升序

sort -u 的话是整行内容的排序后去重

count某一列，sort后|uniq -c|sort

uniq在去重的同时还能把数量统计出来，并输出到第一列