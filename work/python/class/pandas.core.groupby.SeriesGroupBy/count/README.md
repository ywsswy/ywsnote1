df['Age'].value_counts() # 默认降序 可以设置df['Age'].value_counts(ascending = True)
功能上类似 df.groupby('Age')['Age'].count()
统计属性中每个值出现的个数

