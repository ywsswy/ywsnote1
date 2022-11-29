echo "${var}" #-e参数会把转义进行解释，-n会不增加尾换行， ""双引号必须有，从文件读取的东西，不用-e转义输出，但是shell代码里的不可打印字符，要用 -e 输出

# 关于解释转义（区分bash中的处理，和awk中的处理）
echo -en "\0011" # 这是8进制的输出方法？我坚决不用8进制
echo -en "\x09" # 这是16进制的方法，awk中好像只认这种，不认8进制
echo -en "\t" # 这是转义的方法
to_str="\\\[k" # 文件写的是三个\，实际上只有两个字符


# 其他字符串处理
var="runoob is a great site"
echo "${var:2:1}" # 输出 n  #语法是${name:startloc(0s):length}
echo "${var//a/A}" #输出 runoob is A greAt site  #【所有】替换
printf "%x" "'${var:2:1} " #输出某个字符对应的ascii码（机器） （最后有一个空格，因为叹号不会被执行）
printf "%x" 2024 #输出某数字的十六进制
echo $a |LC_ALL=C awk '{printf("%c", $1)}' #输出某ascii码对应的字符 （a要是十进制数字），awk设置locale见awk
echo $((16#FF)) #以某进制来解析某字符串，有了码数，去解码
echo "obase=16;255" |bc #以某进制来表示某数字，编码

# 字符串运算
一切变量皆为字符串？只不过字符串也可以加减乘除
s1="23"
s2="47"
s3=$(expr $s1 + $s2) #这种最保险,乘法*需要转义，但是效率非常低，用$((s1 * s2))快

