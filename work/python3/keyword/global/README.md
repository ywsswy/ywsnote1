在函数内要对函数外的变量赋值需要在函数内声明global；

解释器遇到一个变量或函数时，它会按照LEGB的作用域顺序进行查找，即Local（局部）、Enclosed（嵌套）、Global（全局）和Builtin（内建）。


locals -> enclosing function（该函数嵌套在哪个函数里时） -> globals -> builtins


```
a = 0
b = 0
c = {}

def f1():
    global b
    print(a)  # 不报错，读取（非赋值/定义）操作，可以按照LEGB的作用域顺序进行查找，能成功找到全局变量
    b = b + 1  # 如果不写"global b"会报错，等号左边认为是局部变量b，等号右边认为是全局变量b，自相矛盾了
    b = 1  # 如果不写"global b"不报错，赋值/定义操作，会默认将将其视为当前作用域变量，没找到就是新定义（起不到修改全局b的作用），如果希望指的是全局变量，需要用global声明，或者把变量定义放在全局类中 class YGlobalData(object):
    c["key"] = "value"  # 能起到修改全局变量的效果，因为改的是c里面的内容而不是c本身，如果写c = {"key":"value"}就起不到修改全局b的作用了；

f1()
```
