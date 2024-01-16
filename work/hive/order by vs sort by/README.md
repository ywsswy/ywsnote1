order by 全局排序
order by只会启用一个reduce所以比较耗时,因此order by 是全局的

sort by 局部排序
在每个reducer中是有序的，当设置reducer的个数=1时（set mapreduce.job.reduces=1;）等价于order by，否则的话输出可能只是其中一个reducer的数据，不准确