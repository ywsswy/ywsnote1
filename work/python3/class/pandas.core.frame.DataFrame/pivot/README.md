数据透视表
pivot 不等价于 pivot_table
example_pivot = example.pivot(index = 'Category',columns= 'Month',values = 'Amount')
#index 索引 columns列名 每个单元格显示的值
#不能有任何交叉，因为这个pivot只是把DataFrame转换一个显示格式（结果依然是pandas.core.frame.DataFrame类型），不能出现计算
pivot_table则可以隐含计算
#默认值就是求平均
pt = df.pivot_table(index = 'Sex',columns='Pclass',values='Fare')
df.pivot_table(index = 'Sex',columns='Pclass',values='Fare',aggfunc='max')#指定单元格数值计算方法
df.pivot_table(index = 'Sex',columns='Pclass',values='Fare',aggfunc='count')#计数，类似pandas.crosstab(index = df['Sex'],columns = df['Pclass'])

