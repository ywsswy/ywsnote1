The Zen of Python

快排代码

变量交换 a, b = b, a

遍历 
for i in li:
    do_sth_with(i)

安全关闭文件
with open (path, 'r') as f:
    do_sth_with(f)

尽量用库函数，而不是奇淫语法
print(list(reversed(a)))
优于
print(a[::-1])

格式化输出用'{<name>:<format>}'.format(<name>=<value>)

generators】的特性尤为Pythonic

库/框架的趋势
包和模块命名：小写、单数】、短小
包通常仅作为命名空间，如只包含空的__init__.py文件】


