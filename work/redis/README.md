redis-cli -c -h <ip> -p <port> <cmd>

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

>STRLEN <keyname>
可以看到一个redis的值多长


>set <keyname> "\x00\t\x01hh"
# 二进制的方式



# python的方式
from redis import StrictRedis
r = StrictRedis(host='localhost', port=6379)
r .set('name', 'Bob') 
print(r.get('name'))