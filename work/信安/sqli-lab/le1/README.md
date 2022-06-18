http://127.0.0.1/sqli-labs-master/Less-1/?id=4
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
//仅单层解析，就是只会把$id替换掉，但不会把$id的值再解析一次
例如id = "$aa",上面那句sql不会知道aa的值是多少的
【
$result=mysql_query($sql);//resource(7) of type (mysql result)
mysql_query() 仅对 SELECT，SHOW，EXPLAIN 或 DESCRIBE 语句返回一个资源标识符，如果查询执行不正确则返回 FALSE。
对于其它类型的 SQL 语句，mysql_query() 在执行成功时返回 TRUE，出错时返回 FALSE
对其进行$row = mysql_fetch_array($result);或者mysql_fetch_row()才能得到资源中的数据
$row = mysql_fetch_row() 从结果集中取得一行数据并作为数组返回。
每个结果的列储存在一个数组的单元中，偏移量从 0 开始。
$row[0];$row[1];
【依次调用】 mysql_fetch_row() 将返回结果子集中的下一行（因为limit a,b会返回b-a行），如果没有更多行则返回 FALSE。
mysql_fetch_array() 是 mysql_fetch_row() 的扩展版本。除了将数据以数字索引方式储存在数组中之外，还可以将数据作为关联索引储存，用字段名作为键名。
mysql> select * from users;
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
mysql_fetch_row()
Array
(
    [0] => 1
    [1] => Dumb
    [2] => Dumb
)
mysql_fetch_array()
Array
(
    [0] => 1
    [id] => 1
    [1] => Dumb
    [username] => Dumb
    [2] => Dumb
    [password] => Dumb
)
【
select .... limit a,b
查询结果集中取子集[a,b)
【
mysql有自动转换str的坑
比如，
mysql> select * from users;
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
select * from users where id = '2dm';//这条语句仍然能把id=2的找到
select * from users where id = 2dm;//这条语句就不能了
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=2%27
SELECT * FROM users WHERE id='2'' LIMIT 0,1
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''2'' LIMIT 0,1' at line 1
//这就是单引号没闭合，语法报错
【
$id=$_GET['id'];//$id=$_GET['ID'];二者不同，要区分大小写
【
三种注释写法
#DELETE FROM SeatInformation  
/*DELETE FROM SeatInformation */
-- DELETE FROM SeatInformation
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=1' and 1=1
没闭合报错
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=1'-- and 1=2
成功，因为-- 会注释掉后面的东西
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=1'--+and 1=2
http://127.0.0.1/sqli-labs-master/Less-1/?id=1'--'and 1=2
也成功，为啥--后面没有空格也成功了？
【
浏览器自动url_encode
'%27
 %20
需要手动encode传递
#%23
无encode的有-01ab=?/+
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=1' #
失败，浏览器中#是锚点，加载完后跳转到某锚点，所以这里应该把#手动url_encode
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=1' %23 1=2
成功，把#手动url_encode
【
http://127.0.0.1/sqli-labs-master/Less-1/?id=1' order by 4 %23
报错，order by 4是指把查询结果按第四列排序，可知一共3列
