df = pd.read_csv('./data/titanic.csv')
print(type(df))
#<class 'pandas.core.frame.DataFrame'>
data = pd.read_csv('./data/flowdata.csv',index_col = 0,parse_dates = True)#尝试把不标准的时间类型转换好
