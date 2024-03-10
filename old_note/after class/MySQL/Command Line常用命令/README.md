///////////////////////////
show databases
use [<database name>]
show tables
describe [<table name>]
<==>
show columns from [<table name>];打印表的列属性
Field	Type	Null	Key	Default		Extra
<name>	int<11>	No	PRI	NULL		auto_increacement
	char<12>Yes	UNI
			MUL	
create [<database name>]
source [<.sql file's absolute path>];文件输入sql命令
命令以;或者\g结束
select prod_id,prod_name
from products;
select distinct vend_id
from products;
select prod_name
from products
limit 5;返回行数不超过5
select prod_name
from products
limit 1,4;从第一行开始不超过4行
select products.prod_id,prod_price,prod_name
from sour.products
order by prod_price,prod_name;完全限定名，先按价格排序
select prod_price,prod_name
from products
order by prod_price desc
limit 5;注意order by 和 limit的顺序，desc降序且仅指定其前最近的一列；可用此找最值
【where】
=	<>	!=	<	<=	>	>=	
between .. and ..
is null
and	or	(and 优先级高于 or)	
in	not in
 select prod_name,prod_price
 from products
 where vend_id in (1002,1003)
 order by prod_name;
select prod_id,prod_name
from products
where prod_name like 'jet%';
【like通配符；】
%匹配任意字符任意次（包括0），但不能匹配null；
_匹配任意字符一次
【正则表达式】注：不区分大小写^&
select prod_name
from products
where prod_name regexp '1000'
order by prod_name;
where vend_name regexp '\\.'	匹配特殊字符‘.’，
	\\f	\\n	\\r	\\t	\\v	\\\
select prod_name
from products
where prod_name regexp '[[:digit:]]{4}'
order by prod_name;
类
[:alnum:]
alpha	
blank	space and \\t
cntrl	ASCII 0-31 and 127
digit
graph	any except space
lower	
print	any
punct	except	alnum and cntrl
space	space and \\f\\n\\r\\t\\v
upper
xdigit	hexadecimal num
[[:<:]] start of a word
[[:>:]]	end of a word
【10章，拼接字段170109】
【25章，触发器】
响应delect insert update
只有表有触发器，且一张表至多6个，三个命令前后各一个

