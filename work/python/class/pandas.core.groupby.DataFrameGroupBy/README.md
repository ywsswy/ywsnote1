import pandas as pd
df = pd.DataFrame({'key':['A','B','C','A','B','C','A','B','C'],
                  'data':[0,5,10,5,10,15,10,15,20]})
df.groupby('key').sum()
可以进一步访问series，pandas.core.groupby.DataFrameGroupBy到pandas.core.groupby.SeriesGroupBy
df.groupby('Sex')['Age']
