select at1 '姓名',at2 '学号',at3 '课程',at4 '该课成绩',at5 '该生平均成绩'
from     (select ta1.sname at1,ta1.sno at2,ta2.cname at3,ta3.grade at4
         from    student ta1
                 ,course ta2
                 ,sc ta3
         where   ta1.sno=ta3.sno
                 and ta3.grade>= (select avg(ta4.grade) at6
                                 from    sc ta4
                                 where   ta4.sno=ta3.sno
                                 group by        ta4.sno
                                 )
                 and ta3.cno=ta2.cno
         )ta5
         ,(select        avg(ta6.grade) at5,ta6.sno at7
         from    sc ta6
         group by        ta6.sno
         )ta7
where    at7=at2;
//////////////////////////////////////////////////////////////////////
标准查询关键字执行顺序为 from（join与on关联表属于此）->where->group by->having->order by
left join 是在from范围类所以 先on条件筛选表，然后两表再做left join。
而对于where来说在left join结果再次筛选。
	
select  A.ID as AID, B.ID as BID   
from A left join B 
on A.ID = B.ID 
where B.ID<3
ON与where的使用一定要注意场所：
    （1）：ON后面的筛选条件主要是针对的是关联表【而对于主表刷选条件不适用】。
    （2）：对于主表的筛选条件应放在where后面，不应该放在ON后面。
    （3）：对于关联表我们要区分对待。如果是要条件查询后才连接应该把查询件放置于ON后。
                如果是想在连接完毕后才筛选就应把条件放置于where后面。
    （4）： 对于关联表我们其实可以先做子查询再做join
//group by 后面写的是把这个相同的拿出来每一组相同的就做一次聚集运算
//select from 都设置别名
//select一行就写下
