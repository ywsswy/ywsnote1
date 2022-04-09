echo '11@22@33' |xargs -d '@'  echo 
设置分隔符来变成多个参数

echo "hello" |xargs --replace echo {} "world"

xargs是把标准输入当作参数给后面的命令来执行
如果有--replace则是替换到{}处，而不是尾部，可以重复写多个{}