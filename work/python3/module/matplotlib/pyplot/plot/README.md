plt.plot([1,2,3,4,5],[1,4,9,16,25]) # 画二维数据图，参数可以是list，numpy.ndarray
plt.plot([1,2,3,4,5],[1,4,9,16,25],'--')#默认实线
字符 	类型 	字符 	类型
'-' 	实线 	'--' 	虚线
'-.' 	虚点线 	':' 	点线
'.' 	点 	',' 	像素点
'o' 	圆点 	'v' 	下三角点
'^' 	上三角点 	'<' 	左三角点
'>' 	右三角点 	'1' 	下三叉点
'2' 	上三叉点 	'3' 	左三叉点
'4' 	右三叉点 	's' 	正方点
'p' 	五角点 	'*' 	星形点
'h' 	六边形点1 	'H' 	六边形点2
'+' 	加号点 	'x' 	乘号点
'D' 	实心菱形点 	'd' 	瘦菱形点
'_' 	横线点 		
plt.plot([1,2,3,4,5],[1,4,9,16,25],'-.',color='r')
字符 	颜色
‘b’ 	蓝色，blue
‘g’ 	绿色，green
‘r’ 	红色，red
‘c’ 	青色，cyan
‘m’ 	品红，magenta
‘y’ 	黄色，yellow
‘k’ 	黑色，black
‘w’ 	白色，white
plt.plot([1,2,3,4,5],[1,4,9,16,25],'--r')
连续很多次plot就会画在同一张图上，或者如下用逗号分割
plt.plot(tang_numpy,tang_numpy,'r--',
        tang_numpy,tang_numpy**2,'bs',
        tang_numpy,tang_numpy**3,'go')
【plot参数
label = 'Chong Qing' #每条线的名称，默认不会显示，除非plt.legend()生成图例
linewidth = 3.0
linestyle=':' #拟合线的类型
color='r'
marker = 'o' #有数据的点的类型
markerfacecolor='r'
markersize = 10
alpha = 0.4 #透明度
