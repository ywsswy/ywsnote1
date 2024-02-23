```
>EVALSHA <sha1> numkeys key [key ...] arg [arg ...]
使用sha1校验和来调用脚本（避免传输大量数据），需要指定key的个数
>SCRIPT LOAD <script>
将脚本 script 添加到缓存中，例如SCRIPT LOAD "return 666"，返回值是脚本的 SHA1 校验和，不同集群相同的脚本肯定能计算出相同的sha1校验和
>EVAL <script> numkeys key [key ...] arg [arg ...]
执行lua脚本，调用一次后会自动生成sha1，可以无需手动load生成
```

- script内容示例：
```
local key = KEYS[1];
local expirescore = tonumber(ARGV[1]);  -- 参数都是字符串，对字符串不转换直接进行数值比较是不对的
redis.call('ZREMRANGEBYSCORE', key, -1, expirescore);
return {6, 66}
```