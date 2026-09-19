http://www.cnblogs.com/peterpan0707007/p/7501575.html
https://bbs.pediy.com/thread-220075.htm
https://blog.csdn.net/h330531987/article/details/69495452
相关设置
1）
sql-connections\db-creds.inc 中设置$dbpass ='root';
这里是负责连接数据库的，可以看到用户 密码 以及数据库名：security
2）
text-align不居中，方便使用下面的格式化调试
echo '<pre>';
print_r($row);//var_dump($row);//都可以，print_r不带类型
echo '</pre>';
3）
phpstudy配置中magic_quote_gpc要关掉，不然所有的单引号双引号都会被转义
firefox插件hackbar
