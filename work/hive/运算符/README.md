where a = '1'

不等于
a <> '1'

<=

>=

<

>j

子串匹配
like '%abc%'

not like '%abc%'

包含于几个枚举值内
in ('1978004','1978003')


-- 计算百分比，并保留2位小数
select cast(repeat_pv * 100.0 / total_pv as decimal(10,2)) as pv_res