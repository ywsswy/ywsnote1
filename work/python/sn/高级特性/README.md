【切片（Slice）
取前3个元素
L[0:3]
如果直接操作（L[0:3] = [3]），是在原L上修改

如果赋值给其他变量导致a与L大小不同（a = L[0:3]） ，则是复制出来一个，a改，L不变
如果赋值出来a与L大小相同（a = L[:]），那么相当于同指向，a改，L也变

【迭代
只要作用于一个可迭代对象，for循环就可以正常运行

for key in d:
...     print(key)

from collections import Iterable
>>> isinstance('abc', Iterable) # 判断是否可迭代，【可迭代对象：Iterable】

for i, value in enumerate(['A', 'B', 'C']):#i可获取从0开始的索引
...     print(i, value)
【迭代器
可以被next()函数调用并不断返回下一个值的对象称为【迭代器：Iterator】
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象
Python的for循环本质上就是通过不断调用next()函数实现的，例如：

for x in [1, 2, 3, 4, 5]:
    pass
实际上完全等价于：

# 首先获得Iterator对象:
it = iter([1, 2, 3, 4, 5])
# 循环:
while True:
    try:
        # 获得下一个值:
        x = next(it)
    except StopIteration:
        # 遇到StopIteration就退出循环
        break
【列表生成式
###能简则简
list(range(1, 11))

>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
【生成器：generator
一边循环一边计算以此来节约空间的机制
1）方法1，生成节约
>>> g = (x * x for x in range(10))#只要把一个列表生成式的[]改成()，就创建了一个generator
>>> g
<generator object <genexpr> at 0x1022ef630>
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> for n in g:
...     print(n)
2）方法2，函数节约
比如费波拉契数列，如果用函数，每求一次，则要重新迭代一次，而生成器可以类似中断，yield会返回，下次进入的时候直接从上次yield的位置执行
def fib(max):
    n, a, b = 0, 0, 1
    print('hhh')
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'

for循环调用generator时，发现拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中：

>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
【
