age = df['Age']
print(type(age))
age.index
#获取索引(第一列的那个)范围？
#可以修改
s1.index = ['a','b','d']
s1.rename(index = {'a':'A'},inplace = True)#inplace表明修改原数据
