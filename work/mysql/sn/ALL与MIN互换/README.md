mysql> select at1 '姓名',at2 '年龄'
    -> from
    -> (
    -> select ta1.sname at1,ta1.sage at2
    -> from student ta1
    -> where ta1.sdept<>'CS'
    -> and ta1.sage<
    -> (
    -> select min(ta2.sage) at3
    -> from student ta2
    -> where ta2.sdept='CS'
    -> )
    -> )ta3;
+--------+--------+
| 姓名   | 年龄   |
+--------+--------+
| 王敏   |     18 |
+--------+--------+
1 row in set (0.00 sec)
select at1 '姓名',at2 '年龄'
from
(
select ta1.sname at1,ta1.sage at2
from student ta1
where ta1.sdept<>'CS'
and ta1.sage<all
(select ta2.sage at3
from student ta2
where ta2.sdept='CS'
)
)ta3;
