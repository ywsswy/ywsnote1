Mysql不能先select出同一表中的某些值，再update这个表(在同一语句中)
mysql> select * from Punishing;
+------+-------+
| id   | value |
+------+-------+
| rt   |   720 |
| ss   |     5 |
| se   |     3 |
| sy   |    10 |
| sf   |     5 |
+------+-------+
把rt的值更新为ss的值
mysql> update Punishing set value=(select value from(select value from Punishing where id='ss')t1)where i
d='rt';
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0
要用中间表来传递值
