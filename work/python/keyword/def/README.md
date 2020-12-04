【def
定义函数
【注意
默认参数一定要设为不可变对象



【参数顺序：
必选参数、默认参数、可变参数、命名关键字参数和关键字参数。
【位置参数
就是普通的按顺序的参数，分必选参数和默认参数
【可变参数 
b（tuple类型）把接收到的都放进tuple，所以b接收的参数个数不限
def fun1(a,*b): 
	for i in b:
		print(i)
fun1(1,2,3)#调用1,a = int 1，b = tuple(2,3)
y = [1,2,3]
fun1(*y)#同调用1#星号只有在函数调用的时候才能用，把list/tuple拆分为一个个元素的可变参数

【关键字参数 
b允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict，调用者可以传入任意不受限制的关键字参数
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

>>> person('Bob', 35, city='Beijing')#方法1
name: Bob age: 35 other: {'city': 'Beijing'}

>>> extra = {'city': 'Beijing', 'job': 'Engineer'}#方法2
>>> person('Jack', 24, **extra)

【命名关键字参数

限制关键字参数的名字，就可以用命名关键字参数，例如，只接收且必须接收1个 city和job作为关键字参数（除非设置了默认参数值）
def person(name, age, *, city, job):
    print(name, age, city, job)
和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。

调用
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
