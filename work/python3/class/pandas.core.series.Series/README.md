可以按照索引访问某一数据
相当于一个DataFrame中的一个（列）对象？
+*运算
运算
df['Sex'] == 'male'	#结果是一个bool构成的series
【创建
1）从DataFrame切出来
2）
import pandas
import pandas as pd
data = [10,11,12]
index = ['a','b','c']
s = pd.Series(data = data,index = index)
s
【访问
1）loc
s.loc['b']
2）iloc
s.iloc[1]
3）索引访问
s['a']
【删除
1）索引
del s['a']
2）s.drop(['b','c'],inplace = True)
【显示
每一行是索引+值
