火焰图中从底往上是调用链路
有些函数看不到可能是被内联了

如下图所示，相当于A调用了B和C，B调用了C；
【计算开销要从顶层计算】A的开销是1/3，B的开销是1/6，C的开销是1/2；
```
   |C-|
|--B--|    |--C--|
|-------A--------|
```

命令行中，默认显示的就是顶层开销占比，形如：
```
+ 50.0% C
+ 33.3% A
+ 16.7% B
```
回车键可以展开每一个函数，展开后有两种效果，一种是带'+'（表示有多处调用这个函数，回车键就可以继续展开看每一处来源的占比），另一种是不带'+'的n个并列项（表示的就是这个函数的调用堆栈）；
如上例子全部展开的效果是：
```
- 50.0% C
  - C
     - 66.7% A
       A
     - 33.3% B
       B
       A
- 33.3% A
  A
- 16.7% B
  B
  A
```


## 命令行快捷键：（建议不在screen中避免乱码错位）
? 帮助
/ 查找
/+Enter 退出查找