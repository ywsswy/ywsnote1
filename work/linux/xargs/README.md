echo '11@22@33' |xargs -d '@'  echo 

echo "hello" |xargs --replace echo {} "world"

|是从把前面作为标准输入给后面的命令
| xargs是把前面作为命令行参数给后面的命令