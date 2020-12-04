有错：
select distinct [com_name]
from t_com
minus
select com_name
from t_com
where com_name = '全部显示'
错：
select [com_id],distinct [com_name]
from t_com
where com_name in
(select [com_name],[com_id]
from t_com
where com_name <> 'hh'
order by [com_id]
}
正确：
SELECT DISTINCT com_model
FROM      t_com
WHERE   (com_name <> '全部显示')
SELECT DISTINCT com_name, com_id FROM t_com ORDER BY com_id
++++
SELECT * FROM [t_com] WHERE (([com_accuracy] = ?) AND ([com_add] = ?))
