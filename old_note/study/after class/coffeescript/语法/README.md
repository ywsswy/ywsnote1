### 
块级多行注释
###
# 单行注释
console.log 1
a = true # boolean
myfun = (x,y) -> x+y
youfun = -> console.log "hh"
myfun x,y
youfun() #无参函数调用必须加括号
if a
  console.log typeof(a)
else if
  console.log 'yes'
else
  console.log 'no'
a = ["do", "re", "mi"] # object a[1]='re'
a = {Jagger: "Rock", Elvis: "Roll"} # object a['Jagger']
for a in [10..1] # 左闭右闭
	console.log a
### 
注意事项
* 区分大小写
* 函数要先定义再使用
* 不能自己用var定作用域，第一次出现在哪里，作用域就是哪里
当超过作用域的时候cs会再来一个var
例如如下代码并不能实现1+2+3+4，因为只有一个y，作用域是全局，do_nothin里修改了全局变量
y = 0
do_nothing = (x) -> 
	y = 0
	return x
for i in [1..4]
	z = do_nothing(i)
	y = y + z
	console.log y
但是如下代码却能实现1+2+3+4，因为有两个y，一个全局y，一个do_nothing域的y
do_nothing = (x) -> 
	y = 0
	return x
y = 0
for i in [1..4]
	z = do_nothing(i)
	y = y + z
	console.log y
## type
boolean
number # 整数小数都是这个
object
string
undefined
## operator
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9] 
a  = numbers[0..2] # 数组的切片，属于深拷贝，不会影响原数组
b = numbers[..] # 全部复制
CoffeeScript 会把 == 编译为 ===, 把 != 变异为 !==. 此外, is 编译为 ===, 而 isnt 编译为 !==.
not 可以作为 ! 的 alias 使用.
逻辑操作方面, and 编译为 &&, 而 or 编译为 ||
is	===
isnt	!==
not	!
and	&&
or	||
true, yes, on	true
false, no, off	false
@, this	this
of	in
in	no JS equivalent
a ** b	Math.pow(a, b)
a // b	Math.floor(a / b)
a %% b	(a % b + b) % b（恒正）
% 是有向零取整（并非必须整数）
==

