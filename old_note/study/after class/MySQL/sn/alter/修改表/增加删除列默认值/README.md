//设置默认值
mysql> alter table sc alter column sno set default 'hh';
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
//删除默认值
mysql> alter table sc alter column sno drop default;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
alter table <table name>
alter [column] <column name> 
{set default <value>}|{drop default};
