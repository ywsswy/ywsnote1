【类型】
Char/Text(n)
Memo/Text//备注
TinyInt/Byte//数字（字节）
Short/SmallInt//数字（整形）
Long/Integer
Single
Double/Float
Decimal(m,n)//m为精度，n为数值范围
GUID//数字（同步复制id）
DateTime/Date/Time
Currency/Money
AutoIncrement(m,n)//自动编号，m为初始值，n为步进值
Bit/YesNo
Image//OLE对象
【字段级完整性约束条件1】
Primary key
Not null
Null
check
【操作】
一、创建
CREATE TABLE <表名>(<字段名1><数据类型1>[字段级完整性约束条件1]
[，<字段名2><数据类型2>[字段级完整性约束条件2]] [,…]
[,<字段名n><数据类型n>[字段级完整性约束条件n]])
[,<表级完整性约束条件1>]；
eg:
CREATE  TABLE  雇员(雇员号  SMALLINT Primary  Key,姓名  CHAR(4) 
Not  Null, 性别 CHAR(1),出生日期  DATE,部门  CHAR(20),
   备注  MEMO)
二、修改
ALTER  TABLE <表名>
      [ADD <新字段名><数据类型>
           [字段级完整性约束条件]] 
      [DROP [<字段名>]…]
      [ALTER <字段名><数据类型>]；
eg:
ALTER TABLE 雇员 ADD 职务 CHAR(10)
add:
add 添加
drop 删除
alter 修改
三、删除
delete 
from t_com
where com_id = 36;
四、更新
update t_com 
set com_name = "二极管1"
where com_id = 33;
//多条
update t_com
set com_name = "h1",com_model = "h2"
where com_id = 33
【ADD】
内联接：
SELECT   tb_news.news_id, tb_news.news_ttl, tb_news.news_date, tb_news.news_content, tb_news.news_enable, 
                tb_news.news_order, tb_news.news_cid, list_newsclass.lnc_name
FROM      (tb_news INNER JOIN
                list_newsclass ON tb_news.news_cid = list_newsclass.lnc_id)

