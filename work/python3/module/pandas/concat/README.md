df5 = pd.concat([df2,df4],axis = 1)
连接
df3 = pd.concat([df,df2],axis = 0)
df3
增加列
df2['Tang'] = [10,11]
df2
增加行
df2.loc['h'] = [2,3,4,5]
