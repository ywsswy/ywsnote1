重采样
（索引是时间戳）
data.resample('D').mean()#按照每一天重新计算均值
data.resample('D',how='mean').head()
data.resample('D').max().head()
data.resample('3D').mean().head() #每3天为一类
data.resample('M').mean().head() #月
data.resample('M').mean().plot() #可以画图
