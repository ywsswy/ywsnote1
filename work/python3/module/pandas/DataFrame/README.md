创建
1)
data = {'country':['aaa','bbb','ccc'],
       'population':[10,12,14]}
df_data = pd.DataFrame(data)
df_data
#参数是字典，键（属性列名str），值（该列的各个值列表[]）
2)
df = pd.DataFrame([[1,2,3],[4,5,6]],index = ['a','b'],columns = ['A','B','C'])
df
#第一个参数是list组成元组，一个list是一行数据
