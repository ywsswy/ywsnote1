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