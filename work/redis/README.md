redis-cli -c -h <ip> -p <port> [-a <password>] <cmd> 

特别注意，如果<cmd>是set <key> <binary_data>时，不能写成 set <key> "$(cat <file>)"，因为\x00会丢掉，要写成 -x set <key> < <file>

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

>STRLEN <keyname>
可以看到一个redis的值多长


>set <keyname> "\x00\t\x01hh"
# 二进制的方式



# python的方式
from redis import StrictRedis
r = StrictRedis(host='localhost', port=6379)
r .set('name', 'Bob') 
print(r.get('name'))