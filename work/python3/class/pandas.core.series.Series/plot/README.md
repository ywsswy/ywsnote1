【折线图
s = pd.Series(np.random.rand(10),index = np.arange(0,100,10))
s.plot()
画图，要求%matplotlib inline
【柱状图
import matplotlib.pyplot as plt
fig,axes = plt.subplots(2,1)#画2个图，分布为2行1列
data = pd.Series(np.random.rand(16),index=list('abcdefghijklmnop'))
data.plot(ax = axes[0],kind='bar')#index在x轴
data.plot(ax = axes[1],kind='barh')#index在y轴
2）
df = pd.DataFrame(np.random.rand(6, 4), 
               index = ['one', 'two', 'three', 'four', 'five', 'six'], 
               columns = pd.Index(['A', 'B', 'C', 'D'], name = 'Genus'))
df.head()  
df.plot(kind='bar')
【直方图
<series object>.plot(kind='hist',bins=50)#bins指把区间分50份
【散点图
data.plot.scatter('quarter','realgdp')
pd.scatter_matrix(data,color='g',alpha=0.3)
