////语句
a = (typename(url) = "String") //第一个是赋值，第二个是判断
if 条件 then
真执行语句
else
假执行语句
end if
while 条件
真循环语句
wend
function yf1//function yf1()//function yf1(a,b)
yf1=value//返回值
end function
////study
变量名、函数名和关键字不区分大小写
无返回值的函数调用可以不写括号，参数间用逗号分割
无参数的函数定义可以不写括号
动态语言类型，变量类型可以随时改变
dim在函数内部即为局部变量，不然就成了全局变量
函数不用在前面声明
////运算符
<>
//不等于
=
//相等/赋值
&
//把两个任意类型的东西拼接成字符串
and not or
////命令(必须不带括号的指令)
dim a
//先定义变量才能使用
////函数
delay ms
showmessage a
//a可以是任意类型，都能在屏幕显示，持续时间大概2200ms，新消息能立即覆盖旧消息
swipe x1,y1,x2,y2,ms
tap x,y
touchdown x,y,id
touchup id
getpixelcolor (x,y)
//获取屏幕点颜色值，返回十六进制的"BBGGRR"
getscreenx()
getscreeny()
//获取屏幕分辨率
////模板
function main
end function
function yf1
end function
function ys2 (a,b)
showmessage a&"&"&b
end function
function ys(a)
showmessage a
end function
delay 2200
main
delay 2200

