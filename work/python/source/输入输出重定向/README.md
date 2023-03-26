import sys

old_stdin = sys.stdin
sys.stdin = open('yin1.txt','r', encoding='utf-8') #这种不要多线程运行，因为只允许一个sys.stdin
#sys.stdout = open('yout1.txt','w')
#print('hello')
temp = input() #不会读回车符？
print(temp)
'''
data_list = []
while True:
    try:
        temp = input()
        data_list.append(temp)
    except EOFError:
        break

print(data_list)
'''

print(xxxx, flush=True) #避免在缓冲区未写入文件


jupyter-notebook 不能对sys.stdin重定向，可以对sys.stdout重定向
从键盘输入input()，可以抛出EOFError异常
从文件读取with open() as f: f.read()不会抛出异常

文件如下代码
with open(filename, 'r', encoding='utf-8') as f:
        data = f.readlines()  # 这个含有换行符
        for i in data:
