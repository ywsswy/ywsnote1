echo "${var}" #-e参数是把\r等多个字符按照转义解释成一个字符，-n是不增加行分隔符， ""双引号必须有

# 可以解释成转义的几种方式（区分bash中的处理，和awk中的处理）
echo -en "\0011" # 这是8进制的输出方法？（必须要求\0开始？）我坚决不用8进制
echo -en "\x0d" # 这是16进制的方法，awk中好像只认这种，不认8进制，\x00也能输出
echo -en "\t" # 这是转义的方法
echo -en "\"" # 这是转义的方法

如果要初始化一个含有不可打印字符串的变量，使用a=$(echo -ne "a\a")的方式


# 其他字符串处理
var="runoob is a great site"
echo "${var:2:1}" # 输出 n  #语法是${name:startloc(0s):length} # length可以超过字符串长度
echo "${var//a/A}" # 输出 runoob is A greAt site  #【所有】替换
echo "${var%[i]*}" # runoob is a great s # 匹配一个%后面的部分，把匹配的部分删除掉，通常可以用于浮点数向下取整
printf "%d" "'${var:2:1} "  # 输出某个字符对应的ascii码 （最后有一个空格，因为叹号不会被执行）
printf "%x" 2024  # 以十六进制输出某数字的值
echo $a |LC_ALL=C awk '{printf("%c", $1)}'  #输出某ascii码对应的字符 （a要是十进制数字），awk设置locale见awk
echo $((16#FF)) #以某进制来解析某字符串，有了码数，去解码
echo "obase=16;255" |bc -l  #以某进制来表示某数字，编码

# 字符串运算
一切变量皆为字符串？只不过字符串也可以加减乘除
s1="23"
s2="47"
s3=$(expr $s1 + $s2) #这种最保险,乘法*需要转义，但是效率非常低，用$((s1 * s2))快，但是这两种都只支持整数
浮点数计算需要借助bc
浮点数比较大小
echo "0.31 < .32" |bc -l  # 返回1表示true，0表示false