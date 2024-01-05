#!/usr/bin/env bash
# echo $SHELL 来查看系统默认的shell，有可能是zsh，使用chsh -s /bin/bash修改默认的shell，推荐执行都用bash来执行，因为mac下sh和bash命令是不同的
# 首行注释必加，这样系统才会自动识别，才会变色，才能tab补齐脚本名，还有#!/usr/bin/env python3
# 如果是脚本任意放，只关心人当前目录，则无需特殊处理；（即，如果没有执行cd/pushd等命令，脚本里面都是默认执行目录（而非脚本所在目录）是当前目录
# 如果是脚本位置必须固定，关心脚本所在的目录，则首行应该如下写（readlink牛，能把所有都变成绝对路径，如果写错了就还是当前路径），调试命令是bashdb path/script.sh -- path/script.sh arg1 arg2

```
if [ `basename $0` != 'bashdb' ];then
  arg0=$0
else
  arg0=$1
  shift
fi
thisdir=$(dirname $(readlink -f $arg0))
pushd ${thisdir}
if [ -f ~/.bash_profile ];then
  . ~/.bash_profile
fi
```
# 一个脚本中的cd，不会影响到其他脚本的目录
# 一个脚本中的任何函数内$0都是该脚本名，这是说在执行脚本之前$0就已经被解析好了

"$0" 脚本名字
"$1" 第一个参数
"$@" 所有参数
"$#" 参数个数，不算脚本名
"${#a}" 获取字符串长度
"${LINENO}" 该代码行号
"$$" 脚本的pid，当前脚本的启动进程
res=$(<function name> <param>) 获取命令/函数输出结果，不用``，用这种方式进行函数调用不会让函数内的变量污染全局，否则还会把返回值当作命令执行，切记自己调的函数必用这种方式，然后紧跟着下一行rec=$?获取指令的返回值（脚本中的exit返回的值）在 shell 中， 返回值为零表示成功/真
eval 会先把后面的字符串先解析，再执行
"" 内的命令/$会被解析执行 # echo "$chr" 和 echo $chr 大不相同，如果chr=“*”，前者会变成echo "*"，后者变成 echo *，后者会输出目录下所有文件。。。
'' 内会当作字符串原封不动，很多终端写的命令最好都是用单引号，不然可能被特殊解析
;连接的两个命令是先执行第一个命令，不管第一个命令是否出错都执行下一个命令 && 仅当第一个命令正确执行完毕后，才执行下一个命令
$(($RANDOM%2+1))  #产生1到2之间的随机数，最多到32767

```
if [ ${a} -gt 0 ];then #大于0
elif [ ! -e "file" ];then
  echo "文件不存在" # e是这个名字存在？d是常规文件存在？
elif [ "$s1" = "start" -o "$s1" = "restart" ];then # 在方括号内做字符串使用时要加""，其他时候赋值等操作不需要，使用一个=，而不是==，因为前者兼容性高，如dash；另外如果是做字符串大小比较，需要使用两层括号进而提醒bash不要把>或者<当做重定向符号，并且注意不支持>=和<=
  echo "逻辑或，-a是逻辑与,不能分行写，除非使用\换行"
elif [ "${var+yes}" = "" ];then
  echo "${var} unset" # 变量有三种情况，非空，空，unset，通过判断${var+set}为空，说明unset
else
fi
```

# 函数内的 echo 如果不重定向标准错误，则返回的是函数返回值，return或者函数最后一条命令 是函数的退出状态码；因此不允许函数为空/没有返回码
# 读取文件的写法，这种效率不高，大文件，建议用awk
```
function YRead() {
  myIFS=$(echo -ne "\x0a\x0d")
  while OLDIFS=${IFS}; IFS=${myIFS}; read -r line; ret=$?; IFS=${OLDIFS}; [ $ret -eq 0 ]; do  #这个IFS置空，否则read line会把行首行尾的空白字符忽略掉的~，while的IFS变量会影响整个文件，所以放到【函数】局部中，不过如果要修改全局变量，那就没办法了
    # do something with "$line"
  done < file #不写重定向的话就是从标准输入读取，或者cat file | while ...
}
```

# \<cmd1>; \<cmd2>; | 这里的管道是针对<cmd2>的，如果是多条命令执行完再给管道应该是(<cmd1>; <cmd2>;)| 

# 打日志的方法
```
gEnableLogLevelVec=(
"DEBUG"
"INFO0"
"ERROR"
)

function GetOS() {
  os="$(uname)"
  if [ "$os" = "Darwin" ];then
    echo -n "mac"
  else
    echo -n "linux"
  fi
}

function HashGet() {
  res=$(GetOS)
  if [ "$res" = "mac" ];then
    md5hash=$(echo -n "$1" |md5)
  else
    md5hash=$(echo -n "$1" |md5sum)
  fi
  echo $((16#${md5hash:0:15}))
}

gEnableLogLevelMapk=()

for ((i = 0; i < ${#gEnableLogLevelVec[*]}; ++i))do
  hash_index=$(HashGet "${gEnableLogLevelVec[$i]}")
  gEnableLogLevelMapk[${hash_index}]="${gEnableLogLevelVec[$i]}"
done

function YLog() {
  # $1: $LINENO
  # $2: debug level, DEBUG,INFO0,INFO1,INFO2,WARN,ERROR
  if [ "${gDisableLogLevelCheck+set}" != "" ];then
    echo "$(date +"%Y-%m-%d %H:%M:%S") [$2]: [$arg0:$1]:${*:3}" >&2
  else
    hash_index=$(HashGet "$2")
    if [ "${gEnableLogLevelMapk[${hash_index}]+set}" != "" ];then
      echo "$(date +"%Y-%m-%d %H:%M:%S") [$2]: [$arg0:$1]:${*:3}" >&2
    fi
  fi
}
```

# 终止整个脚本的方法，如果这个脚本是被其他脚本调用的，希望只终止本脚本，则在调用前后分别加上set -m; set +m，给独立进程组
```
function KillScript() {
  # pstree -p $$ |awk -F 'sh\\(' 'BEGIN{RS=")"}{if(NF==2) printf("%s ",$2)}' |xargs kill -9
  $(YLog "$1" ERROR "${*:2}")
  kill 0
  sleep 1
  kill -9 0
}
```

# 数组[强制要求下标0s且连续]的写法，可以任意多空白符，但是=(必须连在一起
# for的写法，i的作用域不局限在for内，死循环可为for ((;;))，可以使用break
# 如果多行代码写成一行，处理do 后面不需要加分号，每一行结尾都加分号
# 二维数组可以使用eval实现，见eval
```
vec=('a' 'b' 'c')  # 命令的结果是数组的话，可以写成 vec=($(ls .)) #这时候只能通过下法输出，不能看$vec，因为这只会输出首地址的字符串；一个字符串可以转成数组vec_s="a b c";vec=($vec_s) ；另外这里的双引号和单引号没有看出来区别
for ((i = 0; i < ${#vec[*]}; ++i))do
  $(YLog $LINENO DEBUG "${vec[$i]}")
  $(YLog $LINENO DEBUG "${#vec[$i]}")
done  # 如果要把所有语句写到一行，需要在done之前加一下分号
```

# map[随机访问]，(${!vec[*]})获取所有的伪key，(为什么要两个结构，因为map的key必须是数值类型，所以这里不管什么类型都hash成数值作为伪key)
# 因为关联数组/map bash 的版本必须 >= 4.1.2，所以通过两个数组来实现，一个存key，一个存value，伪key相同即key做了hash后的数字，如果是set的话，就只用count_k
```
count_k=()
count_v=()
hash_index=$(HashGet "${<key>}")
## 查找
if [ "${count_k[${hash_index}]+set}" != "" ];then
  echo "exist"
else
  echo "not exist"
fi
## 修改/插入
count_k[${hash_index}]="${<key>}"
count_v[${hash_index}]="${<value>}"
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
    :  # 这是占位，啥都不执行的方法
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
