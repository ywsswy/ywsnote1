创建series
import pandas
import pandas as pd
data = [10,11,12]
index = ['a','b','c']
s = pd.Series(data = data,index = index)
s
构造参数可以为list，numpy.ndarray
