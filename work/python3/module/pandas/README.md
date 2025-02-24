【访问
data = {'country':['aaa','bbb','ccc'],
       'population':[10,12,14]}
df_data = pd.DataFrame(data)
 	country 	population
0 	aaa 	10
1 	bbb 	12
2 	ccc 	14
age = df['Age']    #返回pandas.core.series.Series结构（dataframe中的一行/列）
age[:5]
