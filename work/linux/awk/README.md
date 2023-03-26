# new

-v FS中是跟正则有点关系的，所以有些特殊字符要转义，转义的方式比较特别，原来是"["的，转义是需要写"\\\["

-v FS=" "是会把连续的空格视为1个空格，首尾会被忽略，太坑了

awk '{printf("")}' 输出的是ut8编码，如果需要ASCII则要加LC_ALL=C
> echo -n "128" |LC_ALL=C awk '{printf("%c", $1)}' |hexdump -C
00000000  80                                                |.|
00000001
> echo -n "128" |awk '{printf("%c", $1)}' |hexdump -C
00000000  c2 80                                             |..|
00000002

求和命令
cat test.txt |awk 'BEGIN{avg=0;sum=0;num=0} {sum += $1;num++} END{avg=sum/num;printf("num:%d, total:%d, avg:%d\n",num,sum,avg)}'


# old
cat 2.txt |awk '!a[$2]++{print}'   #去除第二列重复的行（重复的仅保留第一个）
- awf -F 的分隔符分割后，得到一个字符串数组[$1,$2,$3....]，然后仅进入【一次，多少行就多少次？】后面的执行语句
- 语句中可以使用的有：（注意$前缀有特殊含义，普通变量不用加$前缀）
- 普通变量也非常智能，有点python动态类型的赶脚，随写随定义，如果希望用到语句外面的变量，可以通过 -v my1=$PATH -v my2=$LIBPATH 的方式在语句中使用my1和my2
- pstree -p $$ |awk -F 'sh\\(' 'BEGIN{RS=")"}{if(NF==2) printf("%s ",$2)}' |xargs kill -9  ##BEGIN{这里设置RS是行结束符}
```
$0 表示“处理后的“全部字符串
$1 表示切割后得到的第一个字符串（如果没有切割成功，$1和$0是等价的）
NR 表示当前是第几行/第几次进（1s）
NF 表示字符串数组的size()，n个分割，NF就是n+1
printf和c语言一样
length($i)字符串字符数
substr($i, start, len) 截取字符串
+-*\

echo '1 2 3' | awk -v FS=' ' -v OFS='_' '{$1=$1;print $0}'

```
echo '中国 你 好了吗 are you interestring' |awk -F " " '{
for(i=1;i<=NF;i++)
{
    if (substr($i, 1, 1) > "\177")
    {   
        utf8=3
    }
    else
    {   
        utf8=1
    }
    if (length($i)*utf8 < 6)
    {   
        printf("%s",$i);
    }
}
printf("\n");}'

# 要截取find出来的有色区域，由白变粉/或者由粉变白，作为分隔符
## 在命令行写由白变粉，按理说是\033[01;31m\033[K，但是awk会做正则处理，所以有关正则的，例如[，要转义\[，二次转义\\\[，最后成了
\033\\\[01;31m\033\\\[K

见 shell/source/替换find到的东西





awk 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt


# 除前两列去重，且统计频次
cat 2 |awk -v FS=' ' '
{$1=""; $2=""; a[$0]++;}
END{
for (i in a){
    printf("%s: %s\n", a[i], i);
}
}
'



cat 2 |awk -v FS=' ' 'BEGIN{
printf("start\n");}
{$1=""; $2=""; sum=sum"\n"$0; printf("%s\n", $0);} #内部做字符串拼接的方式
END{
printf("%s\n", sum);
for (i = 0; i < 3; ++i){ #内部for循环写法
    printf("hello\n") #不支持echo哦亲
}
for (i in a){ #内部数组的遍历方法
    printf("%s\n", a[i]);
}
}'


这内部，如果写了a[9]，哪怕没有赋值，a的length也会增加，即出现即增加，类似c++里map[]的坑
要区分开数值 or 变量， 9 "hi" 是数值， hi是变量