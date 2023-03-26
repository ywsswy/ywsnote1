mysql> update p
    -> set color = '蓝'
    -> where color = '红';
Query OK, 3 rows affected (0.11 sec)
Rows matched: 3  Changed: 3  Warnings: 0
mysql> select * from p;
+------+--------+-------+--------+
| pno  | pname  | color | weight |
+------+--------+-------+--------+
| P1   | 螺母   | 蓝    |     12 |
| P2   | 螺栓   | 绿    |     17 |
| P3   | 螺丝刀 | 蓝    |     14 |
| P4   | 螺丝刀 | 蓝    |     14 |
| P5   | 凸轮   | 蓝    |     40 |
| P6   | 齿轮   | 蓝    |     30 |
+------+--------+-------+--------+
6 rows in set (0.00 sec)
mysql>
