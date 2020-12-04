相同的分在一起
df = pd.DataFrame({'key':['A','B','C','A','B','C','A','B','C'],
                  'data':[0,5,10,5,10,15,10,15,20]})
df.groupby('key').sum() #不加sum方法，则是返回一个pandas.core.groupby.DataFrameGroupBy对象
可以结合numpy的操作
df.groupby('key').aggregate(numpy.sum) # numpy.mean
访问对象的属性
.count()
