df5.drop(['j'],axis=0,inplace = True)#默认是False，所以，一般是获取返回值
# True是直接替换原来的数据，否则原数组内存值不变
# axis=1则是删除列
或者
del df5['Tang']
df5 
