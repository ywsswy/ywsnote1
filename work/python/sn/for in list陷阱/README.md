#最好不要在循环遍历中删除
test_list = ['a','b','c','d','e']

for i in test_list:
    if i == 'c':
        test_list.remove(i)
    print(i)
##输出结果是'a' 'b' 'c' 'e' ，而d没有遍历到




#正确删除法
test_list = ['a','b','c','d','e']

for i in test_list[:]: #这里拷贝一个副本
    if i == 'c':
        test_list.remove(i)
    print(i)
print(test_list)

