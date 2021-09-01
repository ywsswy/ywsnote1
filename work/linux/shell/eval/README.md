正常在脚本里写$res，就会把res当做变量解析出结果的

但是如果一个变量的名字是在运行时确定的，那么就应该先拼接出一个变量名字符串，然后在解析，这时候就要用到eval

首先使用echo和\$（对$转义）拼接出变量名的解析前的写法

例如
a1=3
i=1
varname="\${a${i}}"   # $a1
res=$(eval echo $varname")  # 3

模拟二维数组
nodes1_vec=(
"xxx"
"xxx"
)

nodes2_vec=(
"www"
)

nodes_vec_vec=(
"nodes1_vec"
"nodes2_vec"
)
先遍历nodes_vec_vec找到下一位的名字，然后使用eval拼接再进行遍历