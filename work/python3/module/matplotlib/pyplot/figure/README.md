指定接下来是在第几张图上绘画
默认是plt.figure(1)
cla clf()#清空画布中的Axes
plt.subplot(2,3,1)表示把图标分割成2*3的网格。也可以简写plt.subplot(231)
fig1.add_subplot #这个是位置规范的绘图区域
fig1.add_axes #这个是位置可自己定义的绘图区域
可以来回切换
fig1 = plt.figure(1) #激活1号画布，接下来的绘制操作都是在1号画布上
x = np.linspace(0,np.pi*2,99)
y = np.sin(x)
plt.plot(x,y) #没设置subplot，则默认当前Axes是subplot(111)
plt.title('fig1')
fig2 = plt.figure(5) #激活5号画布
x = np.linspace(0,np.pi*2,99)
y = -np.sin(x)
plt.plot(x,y)
plt.title('fig2')
fig3 = plt.figure(1) #fig3 其实和fig1指向同一个画布
plt.plot(x,y)
//////////////////////////////////////////////////////////////////////
fig1 = plt.figure(1)
plt.title('test1')
x = np.linspace(0,np.pi*2,99)
y = np.sin(x)
z = -y
ax1 = fig1.add_subplot(2,2,4)
ax1.plot(x,y)
ax1 = fig1.add_subplot(2,2,3)
ax1.plot(x,y)
ax1 = fig1.add_subplot(2,2,4)
ax1.plot(x,y)
