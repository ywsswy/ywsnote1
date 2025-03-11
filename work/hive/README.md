HIve的文件存储格式有四种：TEXTFILE 、SEQUENCEFILE、ORC、PARQUET，前面两种是行式存储，后面两种是列式存储；所谓的存储格式就是在Hive建表的时候指定的将表中的数据按照什么样子的存储方式，如果指定了A方式，那么在向表中插入数据的时候，将会使用该方式向HDFS中添加相应的数据类型。

如果为textfile的文件格式，直接load就OK，不需要走MapReduce；如果是其他的类型就需要走MapReduce了，因为其他的类型都涉及到了文件的压缩，这需要借助MapReduce的压缩方式来实现。

总结：
比对三种主流的文件存储格式TEXTFILE 、ORC、PARQUET
压缩比：ORC >  Parquet >  textFile（textfile没有进行压缩）
查询速度：三者几乎一致

## 查看建表语句
show create table xxxx;

## 查询表数据总条数
select count(*) from <table>

## 分区的概念
Hive中，表的每一个分区对应表下的相应目录，所有分区的数据都是存储在对应的目录中。比如wyp表有dt和city两个分区，则对应dt=20131218,city=BJ对应表的目录为/user/hive/warehouse/dt=20131218/city=BJ，所有属于这个分区的数据都存放在这个目录中
show partitions <table> # 查看表的所有分区

## 建表
https://blog.csdn.net/lz_N_one/article/details/126052663
1）先建表，然后把另一张表的查询结果导入到这张表上 insert into insert overwrite
create table <new_t> like <old_t>;
insert into table <new_t> partition (<wtf>) select * from <old_t> limit 30
2）用另一张表的执行结果来建表
DROP TABLE IF EXISTS <new_t>;
create table <new_t> as select * from <old_t> where <wtf>;
3）直接暴力写命令（注意分区表的字段不需要写在前面的字段里）
```
CREATE TABLE my_table (
  id STRING,
  guid STRING,
  queryid STRING,
  type STRING
)
PARTITIONED BY (ds string);
```
然后插入数据，分区字段写在最后
```
INSERT INTO my_db::my_table VALUES ('1', 'a1', '0', '0', '1'),
('2', 'a2', '60', '1', '1'),
('3', 'a2', '60', '0', '1');
```
还有指定分区的插入方式
```
insert into table <table> partition (ds='xxx')
    SELECT xx FROM xx WHERE xx
```
还有把一个分区复制到另一个分区的方法
```
insert into table <table> partition (ds='xxx')
select <other columns> from <table> where ds = 'yyy'
-- 注意这里只能select其他字段，不能把分区字段select进来
```


## 删除分区
ALTER TABLE <table> DROP IF EXISTS PARTITION(<par> = <par_value>);

## 删除表
DROP TABLE IF EXISTS <table>;

## 按某一列去重，显示的就是这一列有哪些种值
select distinct <field> from <table>

## 按某一列去重，显示的就是这一列有几种值（是一个数）
select count(distinct <field>) from <table>

## 内部表和外部表的概念
内部表：元数据库（desc formatted）中（Table Type:）显示的是 MANAGED_TABLEexternal 
外部表：删除表的时候，内部表会删除元数据信息和真实数据信息，外部表只会删除描述信息（1、如果数据已经存储在hdfs上，然后使用hive去进行分析，并且还有可能有其他的计算引擎使用到这份数据，那么请你创建外部表；2、如果一份数据仅仅是hive使用来进行分析，可以创建内部表。）

## 显示元数据
desc formatted <table>
```
Table Parameters:
	numFiles            	1009 # 存储了多少块HDFS文件
	numRows             	46213493 # 总数据条数，但是可能存在记录数未更新的情况，这里就是0
	totalSize           	263429876579 # 总磁盘占用大小（字节）
```