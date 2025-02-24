import matplotlib.pyplot as plt
fig,axes = plt.subplots(2,1)#画2个图，分布为2行1列
data = pd.Series(np.random.rand(16),index=list('abcdefghijklmnop'))
data.plot(ax = axes[0],kind='bar')#index在x轴
data.plot(ax = axes[1],kind='barh')#index在y轴

