cat cmd_file|redis-cli -c -h <ip> -p <port> [-a <password>]
(cat cmd_file;sleep 99999)|telnet <ip> <port> # 这种就不需要安装cli，但是telnet需要保持流，不然会断开，所以要sleep足够的时间，加个ping命令来确保执行完了；尝试过read阻塞，但是没成功。。

特别注意，如果<cmd>是set <key> <binary_data>时，应该把<binary_data>转义，用【单引号】包含进来，例如
```
set test '\x03\x02\x01\x00\x07\x08\x09'
```

>auth <password> # 可以开始的时候不输入密码，这里输入即可
>quit 退出
>ping
PONG 表示连接成功
>get <keyname>
(nil) 表示没有这个key
>set <keyname> <keyvalue> [EX <expire_seconds>]
OK 表示设置成功
>ttl <keyname>
(integer)-1 表示没有过期时间
(integer)-2 表示没有这个key
(nil) 表示没有这个key
其他整数表示还有多少秒存活时间

>TYPE <keyname>
查看类型，有string，set,none
>SCARD <keyname>
如果是set，获取set元素数量
>SMEMBERS <keyname>
如果是set，获取集合全部元素
>SADD <keyname> <value>
如果是set，往set中增加一个元素
>ZADD <keyname> <score> <value>
将value添加到key对应的有序集合（sorted set）里面，并指定sorce，
如果value已经存在，则会更新这个value的score更新到正确的排序位置：https://blog.csdn.net/weixin_62319133/article/details/124317546
>ZCARD <keyname>
如果是有序set，获取set元素数量
>zrange <keyname> 0 -1 withscores
如果是zset，返回全部元素及其分数，奇数行是value，偶数行是score
>ZREVRANGEBYSCORE key max min [WITHSCORES]
如果是zset，返回分支区间内的元素
>del <keyname>
删除key

>STRLEN <keyname>
可以看到一个redis的值多长


>set <keyname> "\x00\t\x01hh"
# 二进制的方式
>info # 查看redis节点信息？vip -> proxy-> redis，这里看到的是redis

>scan 0 match "yyb*" count 10000  # 从游标0扫描匹配某pattern的key，返回下次scan的游标&本次scan的结果，如果游标为0则说明全部scan完了

# python的方式
from redis import StrictRedis
r = StrictRedis(host='localhost', port=6379)
r .set('name', 'Bob') 
print(r.get('name'))