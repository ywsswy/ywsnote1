x = [1000,2000,3000,4000,5000,6000];
y = [976,1951,2926,3902,4877,5853];
plot(x,y)
res = polyfit(x,y,1); # 多项式拟合函数
# 参数1表示最多x^1次，拟合成y = res(1)*x+res(2)的形式，就是res会得出2个值
# 参数2表示最多x^2次，拟合成y = res(1)*x^2 + res(2)*x + res(3)的形式，res会得出3个值

然后可以自己构造一些点，用上面计算出来的res参数，绘制一下最终的曲线，来看效果
xi=1:1:90
yi=polyval(res, xi);
plot(xi,yi)


更灵活的拟合方式：（但是这份数据我没拟合出来）
x = [1;2;3;4;5;6;7;8]
y = [45301725;67706021;84164754;97735921;109176370;120073365;129522041;138330467]
f=fittype('a+b*log(x+c)','independent','x','coefficients',{'a','b','c'})
cfun =fit(x,y,f)
xi=1:1:90
yi=cfun(xi)
plot(xi,yi)


# 其他
X=xlsread('c:\数据.xls','a1:b4');就取出excel表中的第一行到第四行第一列到第二列的所有元素
%X = a1:a10;
%Y = b1:b10;
%cftool
%x = X(1:8)
