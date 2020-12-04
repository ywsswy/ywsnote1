读取
df = pandas.read_csv(.)
自己创建
pandas.DataFrame(data)#参见
访问列
1）label定位
df['Age']	#或者df.Age或者df[['Age']['Name']]某些（不止一列时，就返回的还是DataFrame了）属性列，访问列可以这样，但是访问行不行
df['Age'][:5]	#前5项数据
访问行（元素）
df[:1]
1）loc定位（要先设置好属性df = df.set_index('Name')）
df.loc['a']['A'] 			#某一行的某个属性，这种相当于两次间接查找，是无法赋值的！！！
df.loc['a','A']				#'a'行的'A'属性，这种是一次直接查找，可以赋值
					#所以，最后都加上.copy()用yt1 = yback.loc[0].copy()来赋值
df.loc[0,'name']		#这是索引0，并不是第1个位置
df.loc['Heikkinen, Miss. Laina':'Allen, Mr. William Henry',:]	#可以这种遍历
df.loc['Heikkinen, Miss. Laina','Fare'] = 1000	#赋值
df.loc[df['Sex'] == 'male','Age'].mean()	#bool指定行，'Age'指定列，类似a[3,5]这种二维访问
2）iloc定位（只需要指定位置）
df.iloc[0:5]	#前五行
df.iloc[0:5,1:3]#前五行的属性2，3
3）bool的series访问
df[df['Fare'] > 40][:5]		#df['Fare'] > 40是pandas.core.series.Series类型的
其他）
索引为datestamp时，访问很随意
data[pd.Timestamp('2012-01-01 09:00'):pd.Timestamp('2012-01-01 19:00')]
data[('2012-01-01 09:00'):('2012-01-01 19:00')]
data['2013']
data['2012-01':'2012-03']
data[data.index.month == 1]
data[(data.index.hour > 8) & (data.index.hour <12)]
data.between_time('08:00','12:00')
【运算
二元
cov corr
