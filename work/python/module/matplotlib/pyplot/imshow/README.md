plt.imshow(ynn1,interpolation='nearest',cmap='bone',origin='lower',vmin = 0, vmax = 1)
vmin指定最小值，vmax指定最大值
origin 原点是左上角upper 还是左下角 lower
不然还需要手动翻装轴：
		ax = plt.gca()                       #获取到当前坐标轴信息
		ax.xaxis.set_ticks_position('top')   #将X坐标轴移到上面
		ax.invert_yaxis()
