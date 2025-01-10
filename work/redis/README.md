cat cmd_file |redis-cli -c -h <ip> -p <port> [-a <password>]  # 如果输出到命令行，则会转义可视化；如果输出到文件，则是二进制原始数据（需要按照协议解析，例如list如何解析，zset如何解析），所以推荐输出到命令行可视化，如果想把可视化数据保存到文件，可以使用script命令伪装

~~(cat cmd_file;sleep 99999)|telnet <ip> <port>~~ # 这种就不需要安装cli，但是telnet需要保持连接，不然会断开，但是它本身又不知道服务端是否已经发送完数据，所以只能sleep足够的时间，可以考虑加个ping命令来确保执行完了；尝试过read阻塞，但是没成功。。并且在处理二进制数据会有问题，因为telnet通信的两个方向都采用带内信令方式。字节0xff(十进制的255）叫做IAC（interpret as command，意思是“作为命令来解释”）。该字节后面的一个字节才是命令字节。如果要发送数据255，就必须发送两个连续的字节255，redis服务端并不是telnet，所以二进制中的0xff会被客户端吞掉造成错误解析

特别注意，如果cmd中是set <key> <binary_data>时，应该把<binary_data>转义，整个data用【双引号】包含进来，例如
set a "1 23\x00\x01"
调试阶段不写到cmd_file文件中则是
echo 'set a "1 23\x00\x01"' |redis-cli ...
不能写成 set <key> "$(cat <file>)"，因为经过"$()"之后\x00会丢掉，写成 -x set <key> < <file>则不会丢

>auth <password> # 可以开始的时候不输入密码，这里输入即可
>quit 退出
>ping
PONG 表示连接成功
>get <keyname>
(nil) 表示没有这个key
>set <keyname> <keyvalue> [EX <expire_seconds>] [NX]  # NX 仅当不存在时才能set成功(OK，否则是(nil))，可以用来实现分布式锁
OK 表示设置成功
>ttl <keyname>
(integer)-1 表示没有过期时间
(integer)-2 表示没有这个key
(nil) 表示没有这个key
其他整数表示还有多少秒存活时间

>dbsize
查看key的个数（这个命令可能会被禁用）
>FLUSHALL
清空全部key
>TYPE <keyname>
查看类型，有string,set,none,list
>SCARD <keyname>
如果是set，获取set元素数量
>SMEMBERS <keyname>
如果是set，获取集合全部元素
>SADD <keyname> <value1> [<value2> ...]
如果是set，往set中增加一个元素
>SSCAN key cursor [MATCH pattern] [COUNT count]
如果是set，取出一部分元素，第一次cursor要传0，直到返回0表示查找完毕

>ZADD <keyname> <score> <value>
将value添加到key对应的有序集合（sorted set/zset）里面，并指定sorce（double类型），
如果value已经存在，则会更新这个value的score更新到正确的排序位置：https://blog.csdn.net/weixin_62319133/article/details/124317546
返回值表示元素个数变多多少个
>ZCARD <keyname>
如果是zset，获取元素数量
>zrange <keyname> 0 -1 withscores
如果是zset，返回全部元素及其分数，奇数行是value，偶数行是score
>ZREVRANGEBYSCORE key max min [WITHSCORES]
如果是zset，返回分支区间内的元素，分数从高到低排序
>ZREMRANGEBYSCORE key min max
如果是zset，移除分数位于[min,max]区间的元素
>zscore <keyname> <value>
如果是zset，返回元素的score值

>LPUSH/RPUSH <key> <value>
如果是list，往列表头部（左边）或者尾部（右边）插入元素
>LRANGE <key> <start> <end>
如果是list，返回对应区间[start, end]的元素，0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素

>del <keyname>
删除key
>HSET <key> <field> <value>
如果是hash设置这个key的field的一个value，（hash相当于kkv，有两级key），获取则是HGET
>HMSET <key> <field1> <value1> <field2> <value2>
类似HSET，但是设置多个，，获取则是HMGET


>STRLEN <keyname>
可以看到一个value的大小（字节）

>CLUSTER KEYSLOT <keyname>  # 查看一个key的slot

>set <keyname> "\x00\t\x01hh"
# 二进制的方式
>info # 查看redis节点信息？vip -> proxy-> redis，这里看到的是redis

>scan 0 match "yyb*" count 10000  # 从游标0扫描匹配某pattern的key，返回下次scan的游标&本次scan的结果，如果游标为0则说明全部scan完了

# python的方式
from redis import StrictRedis
r = StrictRedis(host='localhost', port=6379)
r .set('name', 'Bob') 
print(r.get('name'))