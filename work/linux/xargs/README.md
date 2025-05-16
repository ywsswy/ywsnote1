echo '11@22@33' |xargs -d '@'  echo 
设置分隔符来变成多个参数

echo "hello" |xargs --replace echo {} "world"

xargs是把标准输入（逐行）当作参数给后面的命令来执行
如果有--replace则是替换到{}处，而不是尾部，可以重复写多个{}

如果希望逐行处理，并且处理中有管道符，则需要结合bash -c 执行，例如统计文件每一行的空格数并且拼接到行首：
cat file |xargs --replace bash -c 'a=$(echo {} |grep -o " " |wc -l);echo "$a {}"'