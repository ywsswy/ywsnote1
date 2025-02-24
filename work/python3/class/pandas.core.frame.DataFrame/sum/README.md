import pandas as pd
import pandas
df = pd.DataFrame([[1,2,3],[4,5,6]],index = ['a','b'],columns = ['A','B','C'])
df
df.sum() #默认为df.sum(axis = 0)
df.sum(axis = 1)
df.sum(axis = 'columns')

