import numpy as np
import matplotlib.pyplot as plt
plt.plot([1,2,3],[5,7,4])#先plot一堆，后show
plt.show() # 如果要保存图片：plt.savefig('./1.png') plt.clf()

【几种绘制函数
plot 线
bar 条形图
hist 直方图
scatter 散点图
plot_wireframe 线框图（3D立体效果）
'''
【画参数方程就是先有t，然后x=f(t),y=g(t),然后plot(x,y)
【待实验】
https://blog.csdn.net/wizardforcel/article/details/54407212
stackplot 堆叠图
pie 饼图
19章 子图
29章
【example】
import numpy as np
from mpl_toolkits.mplot3d import Axes3D #三维中需要新的轴
import matplotlib.pyplot as plt
#抛物面
x = np.linspace(-5, 5, 11)#size = 11
y = x
x, y = np.meshgrid(x, y) #size = 11*11=121
z = x ** 2 + y ** 2
#print(x)
#print(y)
ax1 = Axes3D(plt.figure())
ax1.plot_wireframe(x, y, z)
ax1.set_xlabel('x axis')
ax1.set_ylabel('y axis')
plt.show()
# 半径为 1 的球
t = np.linspace(0, np.pi * 2, 100)
s = np.linspace(0, np.pi, 100)
t, s = np.meshgrid(t, s)
x = np.cos(t) * np.sin(s)
y = np.sin(t) * np.sin(s)
z = np.cos(s)
ax = Axes3D(plt.figure())
ax.plot_wireframe(x, y, z)
plt.show()
#
x = np.linspace(-5,5,100)
y = np.linspace(-5,5,100)
x,y = np.meshgrid(x,y)
z = x+y
ax = Axes3D(plt.figure())
ax.plot_wireframe(x, y, z)
plt.show()
#elev azim
#z range
