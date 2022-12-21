【不可变
int float str tuple
赋值的时候，是指向另一个新地址

【可变
dict list set 字节数组】
a = b，对a的修改则会引起b的变动

【修改字符串某字符
testStr='hello,ff'
parsedList = list(testStr)
parsedList[6] = 'q'
testStr = ''.join(parsedList)
print(testStr)

testStr = 'Hello'
parsedList = [ch for ch in testStr]
parsedList[3] = 'M'
testStr = ''.join(parsedList)
print(testStr)

【见函数4原则中可变参数不能作为默认参数
【对list等可变对象
list3 = list2[::]切片操作是浅拷贝
list2 = list1 #赋值是浅拷贝，2变，1也变 id(list2) = id(list1)，id是此变量名指向哪里内存

dict2 = dict1.copy() #copy是浅拷贝
dict2 = copy.deepcopy(dict1) # import copy是深拷贝




【函数参数传递，是传值还是传引用？
首先要理解python中【先找内存再贴标签：赋值】的概念
a = 3
不是把这个变量的内存改为什么值
而是先拿出内存，把这个变量指向（贴标签）到内存
作为函数参数，可变类型传递的是引用，不可变类型传递的相当于内容，就是函数中改变不会引起主调的变传对象的引用？

【先操作再赋值】
a[2] = 4
【
变量生存周期并不是调用完就结束，而是要看引用次数

【
a2 = [1,2]

a2.append(4) 
#等价于 a2 += [4]
#不等价于 a2+[4]


