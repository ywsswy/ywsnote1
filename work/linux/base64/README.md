编码：
$ echo -n "Hello World" | base64

-w 0 可以不换行

解码：
$ echo -n "SGVsbG8gV29ybGQK" | base64 -d  # 不管加不加-n，不管是不是多行内容，base64都只处理明文部分，把每一行解码后的数据拼接到上一行尾，所以如果希望解码后也是多行则必须在编码前就把行分隔符号编码进来，或者 cat file |perl -pe 's/(.*)$/$1Cg==/' |base64 -d


可以直接从标准输入读，不担心特殊字符
echo -ne "\x01\x00\x02\x0a\x0d" |base64