select distinct at1 '学生姓名'
from
(        select ta1.sname at1,ta1.sno at2
         from student ta1
)ta2
inner join
(        select ta3.sno at3
         from sc ta3
         where ta3.cno='1'
)ta4
on at2=at3;
//////////////////////////////////////////////////////////////////////
查询所有选修了1号课的学生
//////////////////////////////////////////////////////////////////////
select at1 '学生姓名'
from
(        select ta1.sname at1
         from student ta1
         where exists
         (       select *
                 from sc ta2
                 where ta2.sno=ta1.sno
                 and ta2.cno='1'
         )
)ta3;
//////////////////////////////////////////////////////////////////////
select sname
from sc,student
where sc.cno='1'
and sc.sno=student.sno;
//这个最low
