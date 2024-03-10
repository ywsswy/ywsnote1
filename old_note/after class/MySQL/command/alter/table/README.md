alter table yws201521 modify name varchar(16);
//列的新属性
alter table coursebb add column Bb5 varchar(10) not null,add primary key (Bb5);
//添加主码
alter table coursebb add column bb4 varchar(20) not null default '系统测试值';

