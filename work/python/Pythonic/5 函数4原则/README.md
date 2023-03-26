嵌套缩进尽量不超过3层

考虑向下兼容，如果改了函数中增加了参数，最好增加的那些都是有默认参数的

一个函数只做一件事

不要把函数中默认值定义为可变对象
def testFuc(name,cource=[]):
每次调用都是用的同一个内存的cource（默认参数调用只评估一次）
所以如果修改后，再调用这个函数时进入的cource已经不为空了
正确写法

def testFuc(name,cource=None):
    if cource is None: cource=[]

使用异常替换返回错误】


