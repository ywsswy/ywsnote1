mysql> desc example6;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int(11)     | NO   | PRI | NULL    | auto_increment |
| stu_id | int(11)     | YES  | UNI | NULL    |                |
| name   | varchar(20) | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
mysql> alter table example6 change column name NAME varchar(20) not null first;
Query OK, 4 rows affected (0.07 sec)
Records: 4  Duplicates: 0  Warnings: 0
mysql> desc example6;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| NAME   | varchar(20) | NO   |     | NULL    |                |
| id     | int(11)     | NO   | PRI | NULL    | auto_increment |
| stu_id | int(11)     | YES  | UNI | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
mysql> alter table example6 change column NAME name varchar(20) not null default 'Gary' after stu_id;
Query OK, 4 rows affected (0.09 sec)
Records: 4  Duplicates: 0  Warnings: 0
mysql> desc example6;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int(11)     | NO   | PRI | NULL    | auto_increment |
| stu_id | int(11)     | YES  | UNI | NULL    |                |
| name   | varchar(20) | NO   |     | Gary    |                |
+--------+-------------+------+-----+---------+----------------+
alter table <table name>
change [column] <origin column name> <new column name> <new type> {not null}|{default <default value>}|{first}|{after <later column name>};
alter table example6 modify column name varchar(20) not null after stu_id;
//modify只能改类型不能改名字，其他和change同功能
【注】
列的重命名、列类型的变更以及列位置的移动，重命名、类型为必选项
after
change
first
