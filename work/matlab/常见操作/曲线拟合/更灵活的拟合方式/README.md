x = [1 ;2 ;3 ;4 ;5 ;6 ;7 ;8]
y = [45301725 ;67706021 ;84164754 ;97735921 ;109176370 ;120073365 ;129522041 ;138330467]
y2 = y/10000000 # 从经验来看，如果x和y的量级差距太大有可能参数寻优会出问题，所以最好缩小到差不多的量级

cftool  # 弹出工具窗
```
1）select data：设置x轴和y轴使用哪个变量（这里y轴使用y2）
2）设置拟合方式：custom equation
3）在fit option里设置想拟合的函数参数：例如
a*log(x+b)+c  # 越少越好，能用+-尽量不要用乘除，例如这个不建议换成a*log(x*b)+c
4）在fit option里设置参数初始值（会影响参数寻优，如果默认计算的比较离谱，最好自己设置一下预期的初始a\b\c的值）

然后下面就会输出拟合的a\b\c的值，同时fit plot还是实时显示绘制的曲线
如果发现曲线不显示或者比较离谱，就调整上面所说的y的量级以及a/b/c的初始值即可
```

