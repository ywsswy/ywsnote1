break
continue
【is 可以判断id是否相同
in
not in
【if
if p1 == "1":
  print "http://jwzx.cqupt.congm.in"
elif p1 == "2":#可选
  print "http://jwzx.host.congm.in:88"
else:
  print type

条件表达式
a = b if c else d

【for
#!/usr/bin/python
#filename:test.py
for i in range(1,5): #左闭右开
    print(r'''%d'''%(i))
else:
    print(r'''The for loop is over.''')
print(r'''Here is the end.''')

//for可以有一个终止执行的else
//for in循环是用来循环特定数据结构的，比如list，dict，tuple

data = ['1.3','3','4']
[float(x) for x in data]
#[1.3, 3.0, 4.0]

【while
running = True
while running:
  running = False
else:#else 可选



【lambda
print((lambda x, y: x + y)(2, 5))
【with as [another name]
with open('./data/in2.txt','w') as f:
    f.write('yws' + '\n')
    
txt = open('./data/in2.txt')
d = txt.readlines()
print(d)

【try except
try:
	# there is code to raise error
except Exception as mye:  # 不仅判断异常类型，还把异常信息赋值给mye，老版本python的写法是except Exceptino, mye:
        print('error:{}'.format(mye))
except:

else:#可选，无异常是执行

finally:#可选，有异常也要保证在异常之前处理的部分，没异常就最后执行
ValueError



except Exception as mye: #输出异常信息
