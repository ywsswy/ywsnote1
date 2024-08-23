同一层的执行先后顺序：
from
where
group by
select
order by
limit & offset  # (翻页偏移)

拼写hiveQL的时候，如果把group by写在where前面会报错的；
