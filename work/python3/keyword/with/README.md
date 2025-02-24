with open('./test.txt', 'w') as f:
    f.write('Hello, world!')

with open('E:/Data/Server/train/conf.html', 'w') as f:
    f.write('tomcat')

with open('E:/Soft/Dev-Cpp/yg.txt', 'r') as f:
    num = int(f.read())


with <表达式1> as <变量1>:
    # do something with <变量1>

等价于<变量1> = <表达式1>
只不过with可以在语句块结束的时候调用<变量1>.__exit__()方法



### 
（1）打开文件的例子 
with-as语句最常见的一个用法是打开文件的操作，如下：

with open("decorator.py") as file:
    print file.readlines()

（2）自定义 
with语句后面的对象必须要有__enter__和__exit__方法，如下是一个自定义的例子：

class WithTest():
    def __init__(self,name):
        self.name = name
        pass

    def __enter__(self):
        print "This is enter function"
        return self 

    def __exit__(self,e_t,e_v,t_b):
        print "Now, you are exit"

    def playNow(self):
        print "Now, I am playing"

print "**********"
with WithTest("coolboy") as test:
    print type(test)
    test.playNow() 
    print test.name
print "**********"
上述代码运行的结果如下：

**********
This is enter function
<type 'instance'>
Now, I am playing
coolboy
Now, you are exit
**********

