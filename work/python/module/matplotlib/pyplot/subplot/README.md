画子图
# 211 表示一会要画的图是2行一列的 最后一个1表示的是子图当中的第1个图
plt.subplot(211)
plt.plot(x,y,color='r')
# 212 表示一会要画的图是2行一列的 最后一个1表示的是子图当中的第2个图
plt.subplot(212)
plt.plot(x,y,color='b')
//////////////////////////////////////////////////////////////////////
先subplots指定格子
然后在画每个格子之前，subplot指定位置
最后整体show
def showFreeTime(yn1):
	fig,axes = plt.subplots(5,4)
	for i in range(0,g_week) :
		ynn1 = yn1[:,:,i]
		#print(help(plt.imshow))
		#print(help(plt.subplot))#
		plt.subplot(5,4,i+1)
		plt.ylabel('''week {}'''.format(i+1),fontsize = 8)
		plt.imshow(ynn1,interpolation='nearest',cmap='bone',origin='lower')
		
		ax = plt.gca()                       #获取到当前坐标轴信息
		ax.xaxis.set_ticks_position('top')   #将X坐标轴移到上面
		ax.invert_yaxis()
	plt.show()
	return None
