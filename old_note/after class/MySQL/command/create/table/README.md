mysql> create table s(
    -> sno char(9),
    -> sname char(9),
    -> status int,
    -> city char(9)
    -> );
Query OK, 0 rows affected (0.08 sec)
mysql>
create table ta
(sno char(9)
,cno char(9)
,primary key(sno,cno)
);

