<table>表有两个字段guid，doclist
相同guid的合并（分组），然后合并时把doclist使用空格拼接起来

SELECT guid, concat_ws(' ',collect_list(doclist)) as doclist from <table> group by guid;

-- concat_ws是一种简写，等价于concat(doclist第一个元素, ' ', doclist第二个元素, ' ', ……)


上面的拼接是无序的，如果希望collect_list(doclist)是按照某个字段(ftime)的顺序来排序的话，可以使用
```
SELECT guid, concat_ws(' ', collect_list(COALESCE(docids, ''))) as docids
FROM (
    select guid, docids
    from <table>
    distribute by guid sort by guid, ftime DESC
)
group by guid;
```
-- COALESCE是返回参数中第一个非NULL的值，用于避免处理空值（上面就是空值返回''）

这样的话，相当于先把相同guid的按照ftime排序了，然后再进行拼接；
这个语句中提效的方法在于distribute by <a> sort by <a>, <b>，
区别于order by的全局排序，distribute by + sort by方法中被distribute by设定的字段为KEY，数据会被HASH分发到不同的reducer机器上，然后sort by会对同一个reducer机器上的每组数据进行局部排序；

ps：Hive基于HADOOP来执行分布式程序的，和普通单机程序不同的一个特点就是最终的数据会产生多个子文件，每个reducer节点都会处理partition给自己的那份数据产生结果文件，这导致了在HADOOP环境下很难对数据进行全局排序，如果在HADOOP上进行order by全排序，会导致所有的数据集中在一台reducer节点上，然后进行排序，这样很可能会超过单个节点的磁盘和内存存储能力导致任务失败

-- COLLECT_SET 可以生成去重的数组
-- SORT_ARRAY(COLLECT_SET 可以去重后再排序