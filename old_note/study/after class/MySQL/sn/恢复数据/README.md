https://blog.csdn.net/zhchs2012/article/details/79013951
1）ccftraind数据库里创建同名表，属性个数可以通过frm文件查看（如果不知道，则随便建立一个，重启mysql查看日志错误信息来知道有多少属性列）
CREATE TABLE sproblem(a1 int,a2 int)ENGINE=InnoDB;
2）frm文件覆盖，然后修改/添加根目录的myini里[mysqld]下的
innodb_force_recovery=6
3）重启mysql，就能发现还原出表结构
desc sproblem;
4）导出表结构
mysqldump -uroot -proot ccftraind sproblem > D:\sproblem.sql
5）重启mysql
innodb_force_recovery=0
6）删除表，再用此sql创建表
7）使当前.ibd的数据文件和.frm分离
ALTER TABLE sproblem DISCARD TABLESPACE;
7）ibd文件覆盖，然后，完毕
ALTER TABLE weibo_tweets IMPORT TABLESPACE;
select count(*) from trainproblemstatus;
select * from trainproblemstatus order by itemid desc limit 0,10;
