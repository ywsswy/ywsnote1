加密：
$ echo -n "Hello World" | base64

-w 0 可以不换行

解密：
$ echo -n "SGVsbG8gV29ybGQK" | base64 -d


可以直接从标准输入读，不担心特殊字符
echo -ne "\x01\x00\x02\x0a\x0d" |base64