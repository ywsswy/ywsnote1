#!/usr/bin/env bash

# 切记，自己调的函数必加res=$() ，紧跟着下一行rec=$?，不然会把返回值当作命令执行了
# 首行注释必加，这样系统才会自动识别，才会变色，才能tab补齐脚本名，还有#!/usr/bin/env python3
# 如果是脚本任意放，只关心当前目录，则无需特殊处理；（即，如果没有执行cd/pushd等命令，脚本里面都是默认执行目录（而非脚本所在目录）是当前目录
# 如果是脚本位置必须固定，关心脚本所在的目录，则首行应该如下写（readlink牛，能把所有都变成绝对路径，如果写错了就还是当前路径），调试命令是bashdb path/script.sh -- path/script.sh arg1 arg2
# echo $SHELL 来查看系统默认的shell，有可能是zsh，使用chsh -s /bin/bash修改默认的shell
```
if [ `basename $0` != 'bashdb' ];then
    arg0=$0
else
    arg0=$1
    shift
fi
thisdir=$(dirname $(readlink -f $arg0))
pushd ${thisdir}
```
# 一个脚本中的cd，不会影响到其他脚本的目录
# 一个脚本中的任何函数内$0都是该脚本名，这是说在执行脚本之前$0就已经被解析好了

"$0" 脚本名字
"$1" 第一个参数
"$#" 参数个数，不算脚本名
"$?" 上一条指令的返回值（脚本中的exit返回的值）在 shell 中， 返回值为零表示成功/真
"${#a}" 获取字符串长度
"${LINENO}" 该代码行号
"$$" 脚本的pid，当前脚本的启动进程
res=$() 获取命令/函数输出结果，不用``
eval 会先把后面的字符串先解析，再执行
"" 内的命令会被解析执行
'' 内会当作字符串原封不动
$(($RANDOM%2+1))  #产生1到2之间的随机数

环境变量：不export了话，这个变量只能在当前shell下使用，在shell的子进程中无法使用

```
if [ ${a} -gt 0 ];then #大于0
elif [ ! -e "file" ];then
  echo "文件不存在" # e是这个名字存在？d是常规文件存在？
elif [ "$s1" == "start" -o "$s1" == "restart" ];then
  echo "逻辑或，-a是逻辑与,不能分行写，除非\换行"
elif [ "${var+yes}" == "" ];then
  echo "${var} unset" # 变量有三种情况，非空，空，unset，通过判断${var+set}为空，说明unset
else
fi
```

# 读取文件的写法，这种效率不高，大文件，建议用awk
```
function YRead()
{
    while IFS= ;read -r line;do  #这个IFS置空，否则read line会把行首行尾的空白字符忽略掉的~，while的IFS变量会影响整个文件，所以放到函数局部中
        # do something with "$line"
    done < file #不写重定向的话就是从标准输入读取，或者command | while ...
}
```

# 打日志的方法
```
function HashGet()
{
    md5hash=$(echo -n "$1" |md5sum)
    echo $((16#${md5hash:0:15}))
}

gEnableLogLevelVec=(
"DEBUG"
"INFO0"
"ERROR"
)

gEnableLogLevelMapk=()

for ((i = 0; i < ${#gEnableLogLevelVec[*]}; ++i))do
    hash_index=$(HashGet "${gEnableLogLevelVec[$i]}")
    gEnableLogLevelMapk[${hash_index}]="${gEnableLogLevelVec[$i]}"
done

function YLog()
{
    if [ "${gDisableLogLevelCheck+set}" != "" ];then
        echo "$(date +"%Y-%m-%d %H:%M:%S") [$2]: [$arg0:$1]:${*:3}" >&2
    else
        hash_index=$(HashGet "$2")
        if [ "${gEnableLogLevelMapk[${hash_index}]+set}" != "" ];then
            echo "$(date +"%Y-%m-%d %H:%M:%S") [$2]: [$arg0:$1]:${*:3}" >&2
        fi
    fi
    # 函数内的 echo 如果不重定向标准错误，则返回的是函数返回值，return或者函数最后一条命令 是函数的退出状态码；因此不允许函数为空/没有返回码
    # echo "[$(date +"%Y-%m-%d %H:%M:%S.%N")] [$2] [$arg0:$1] ${*:3}" >&2
}

```

# 终止整个脚本的方法，如果这个脚本是被其他脚本调用的，希望只终止本脚本，则在调用前后分别加上set -m; set +m，给独立进程组
```
function KillScript()
{
    # pstree -p $$ |awk -F 'sh\\(' 'BEGIN{RS=")"}{if(NF==2) printf("%s ",$2)}' |xargs kill -9
    # 函数调用和参数传递的方法，这种方法不会让f1的变量污染全局
    $(YLog "$1" ERROR "${*:2}") # 普通地方是$(YLog $LINENO DEBUG "value")
    kill 0
    sleep 1
    kill -9 0
}
```

# 数组[强制要求下标0s且连续]的写法，可以任意多空格和回车，但是=(必须连在一起
```
# for的写法，i的作用域不局限在for内，死循环可为for ((;;))
# 如果多行代码写成一行，处理do 后面不需要加分号，每一行结尾都加分号
vec=('a' 'b' 'c')  # 命令的结果是数组的话，可以写成 vec=($(ls .)) #这时候只能通过下法输出，不能看$vec，因为这只会输出首地址的字符串，而且一个字符串可以转成数组vec_s="a b c";vec=($vec_s)
for ((i = 0; i < ${#vec[*]}; ++i))do
    $(YLog $LINENO DEBUG "${vec[$i]}")
    $(YLog $LINENO DEBUG "${#vec[$i]}")
done
```

# map[随机访问]，(${!vec[*]})获取所有的伪key，
# 因为关联数组/map bash 的版本必须 >= 4.1.2，所以通过两个数组来实现，一个存key，一个存value，伪key相同，都为key做了hash后的数字
```
count_k=()
count_v=()
## 查找
function HashGet()
{
    md5hash=$(echo -n "$1" |md5sum)
    echo $((16#${md5hash:0:15}))
}
hash_index=$(HashGet ${"<key>"})
if [ "${count_k[${hash_index}]+set}" != "" ];then
  echo "exist"
else
  echo "not exist"
fi
## 修改/插入
count_k[${hash_index}]="<key>"
count_v[${hash_index}]="<value>"
## 遍历
count_index=(${!count_k[*]})
for ((i = 0; i < ${#count_index[*]}; ++i))do
    index=${count_index[$i]}
    echo -n "${count_k[$index]}: "
    echo "${count_v[$index]}"
done
```









# 其他
sh -x 不一定能显示所有的东西，如果脚本内，有这种写法：
```
(test -z "$a") 2>/dev/null
```
那也不会显示的



















# 以下全是注释的写法，多行注释的方法
```
if [ 0 ];then # shell里0也是true，只有空字符串/不存在/未赋值变量才为false
    :
else
   这里不允许写括号
fi

:'
   这里不准写单引号
'

#!/bin/bash
echo "Say Something"
:<<COMMENT
    这里不能写奇数个反引号
    your comment 1
    comment 2
    hello (nihao)
    blah
COMMENT
echo "Do something else"

#!/bin/bash
:<<'COMMENT' #前面要加冒号，是保证这一行的任何结果都执行到:里面了。。。。
这里好像什么都可以写
    echo -en "$line" |awk -v FS="\033\\\[K|\033\\\[m\033\\\[K" -v OFS="_" '{$1=$1; print $2}')
    $(echo -en "$line" |awk -v FS="\033\\\[K|\033\\\[m\033\\\[K" -v OFS="_" '{$1=$1; print $10}'
    `pwd`
`helll
    if [ "$from_str" != ""\
        -a "$file_name" != "" ];then
        from_str=${from_str//\//\\\/}
        sed -i "s/${from_str}/${to_str}/g" "$file_name"
    fi
COMMENT



```

## 裸写、``、$()好像不太一样