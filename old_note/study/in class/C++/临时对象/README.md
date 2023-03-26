1】利用构造函数（显式隐式都可以）
可以不创建有名的对象
而使用这种方式
cout<<Line(Point(1)， Point (4)) .getLen () <<end1; 
创建一个 Point 的临时对象
//////////////////////////////////////////////////////////////////////
Point(4)也可以看成是显式类型转换
一个构造函数，只要可以用一个参数调用，那么它就设定了一种从参数类型到这个类类型的类型转换。
cout<<Line (1 , 4) .getLen() <<end1;
cout<<Line((Point)l, (Point)4) .getLen()<<end1
cout<< Line (static cast< point> (1) ), static cast< point> (4) ) . getLen ()<< end1;
2】只允许显式
。只要在构造函数前加上 explicit 关键字，以这个构造函数定义的类型转换，只能通过显式转换的方式完成。就像这样:
exp1icit Point (int xx=O , int yy=O);
cout<<Line((Point)l, (Point)4) .getLen()<<end1;
cout<< Line (static cast< point> (1) ), static cast< point> (4) ) . getLen ()<< end1;

