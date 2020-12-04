//消除重复记录的同时又能取多列数据
select com_id,com_name
from t_com
where com_id in 
(
select max(com_id)
from t_com
group by com_name
)
order by com_id;

